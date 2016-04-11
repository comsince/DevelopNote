---
layout: post
title: "第三方PushSDK 设计实践"
description: 关于推送的第三方实现由很多，百度云推送，极光推送，个推，友盟推送，之前已经对这几个推送的具体原理和接入方式有过说明，app为了及时获取到服务器端的消息更新，一般会采用轮寻或者推送的方式来获取消息更新，轮寻导致终端设备流量、电量、等系统资源严重浪费，所以目前采用的比较广泛的是推送的方式，目前 Meizu 的 Push SDK 不能脱离 Flyme OS 存在，当该 SDK 脱离 Flyme OS 之后由于没有长链接导致不能正常收到推送消息。本 SDK 首先要解决的时长链接由 SDK 自己维护，同时还要解决的就是多个 app 引用同一个 SDK 时长链接的复用问题。
category: blog
---


## 一. 概述
   * 关于推送的第三方实现由很多，百度云推送，极光推送，个推，友盟推送，之前已经对这几个推送的具体原理和接入方式有过说明，具体详见[各平台推送接入分析]()
   * 推送本质就是与push服务保持一种有效的长连接，以期实时将消息推送到手机客户端，本文也将对推送实现的各个环节遇到的问题进行分析。
   * 鉴于个人正在研究推送相关的项目，所以针对具体的实现做一些简要的分析

## 二. 推送应用处理的问题

本部分主要讨论SDK开发过程中，Android应用集成中的问题

### 2.1 SDK集成问题

SDK开发主要让各个应用以代码集成的方式接入，在传统的开放模式中，应用要接入第三方SDK，必然带回来一些默认的配置，比如AndroidManifest.xml的配置，如下

       <receiver android:name="com.meizu.cloud.PushMsgReceiver">
            <intent-filter>
                <!-- 接收push消息 -->
                <action android:name="com.meizu.flyme.push.intent.MESSAGE" />
                <!-- 接收register消息-->
                <action android:name="com.meizu.flyme.push.intent.REGISTER.FEEDBACK"/>
                <!-- 接收unregister消息-->
                <action android:name="com.meizu.flyme.push.intent.UNREGISTER.FEEDBACK"/>
                <action android:name="com.meizu.c2dm.intent.REGISTRATION" />
                <action android:name="com.meizu.c2dm.intent.RECEIVE" />
                <category android:name="com.meizu.pushdemo"></category>
            </intent-filter>

        </receiver>

除了以上的问题外，SDK接入有可能包括第三方so库的集成，这些都需要用户手动导入，鉴于以上给第三方应用带来极大困扰的集成方式，我们选择aar的集成方式

* 解决方案

 目前我们全部采用Gradle的编译方式，所有的类库都采用Android Studio library工程的目录结构进行组织，这样做的好处包括，Android开发应用无需关心so库，AndroidManifest集成，proguard混淆问题，应用只需要应用aar包就可以，同时aar包也可以发布到artifactory上，这样应用就可以像下面的方式引用即可：

	    compile 'com.meizu.cloud.pushsdk:open:3.0.2-beta@aar'
	    proguard 'com.meizu.cloud.pushsdk:open:3.0.2-beta@pro'
	    compile 'com.meizu.cloud.pushsdk:internal:3.0.3-beta@aar'
	    proguard 'com.meizu.cloud.pushsdk:internal:3.0.3-beta@pro'



### 2.1 SDK单进程单Service实现

#### 2.1.1 通道连接的持久性

  SDK需要与push服务保持长连接，一种普遍的做法是采用Android Service的方式常驻，这样就能保证SDK与push服务保持一种长久的连接

#### 2.1.2 通道的唯一性保证

  出于对手机电量的考虑以及push服务器的承载能力，一般情况PushService一般在一台手机上只能默认挂载到一个应用上，只有这样才能保持Service的唯一性


	Intent pushServiceIntent = new Intent(PushConstants.MZ_PUSH_ON_START_PUSH_REGISTER);
	pushServiceIntent.setClassName(context.getPackageName(),"com.meizu.cloud.pushsdk.pushservice.MzPushService");
	pushServiceIntent.putExtra(PushConstants.REGISTER_PACKAGE_NAME,context.getPackageName());
	context.startService(pushServiceIntent);

 
#### 2.1.3 PushService包名选择

以上讨论了push服务的唯一性，唯一性必然解决的问题是改PushService挂载在哪一个包名下，在Flyme集成的下，这个service组件是默认有云服务挂载，所以在Flyme上所有接入的push的应用的消息都会从这个Service收到转发消息

#### 2.1.4 第三方ROM适配问题

  PushSDK由于还要适配第三方ROM，就必然面临第三方SDK所面临的问题，当所有集成pushSDK的应用同时运行在一个手机上，我们必然有一种优先级算法决PushService到底挂载在哪个应用之上

* 解决方案
我们采用的初级方案是根据应用的包名的顺序优先级最高的应用启动该应用下的PushService，当然为了防止恶意的伪造service，我们还建立了PushService之间的相互校验规则

### 2.2 应用消息到达率

提升消息到达率，保证消息准确到应用

* 发送纯广播
  通过广播传递消息有可能应用退出之后无法收到广播消息

* 通道Service转发消息
  为了解决单纯的广播转发消息，我们设计一种通过Service转发消息的办法，这样办法在百度云推送上也使用过，具体做法如下：

```
  通过调用NotificationService->反射Receiver的OnReceive方法
```
  有时可以方式这种方式出现异常，可以结合广播的方式共同使用，保证消息到达率

以下是NotificationServcie的代码:

	package com.meizu.cloud.pushsdk;

	import android.app.IntentService;
	import android.content.Context;
	import android.content.Intent;
	import android.content.pm.PackageManager;
	import android.content.pm.ResolveInfo;
	import android.text.TextUtils;
	import android.util.Log;


	import com.meizu.cloud.pushsdk.util.DebugLogger;

	import java.lang.reflect.Constructor;
	import java.lang.reflect.Method;
	import java.util.List;

	/**
	 * Created by comsince on 15-6-16.
	 */
	public class NotificationService extends IntentService {
	    private final static String TAG = "NotificationService";

	    public NotificationService(String name) {
		super(name);
	    }

	    public NotificationService(){
		super(TAG);
	    }

	    @Override
	    public void onDestroy() {
		Log.i(TAG, "NotificationService destroy");
		super.onDestroy();
	    }

	    @Override
	    protected void onHandleIntent(Intent intent) {
		if(intent != null){
		    DebugLogger.i(TAG, "onHandleIntentaction " + intent.getAction());
		    String commandType = intent.getStringExtra("command_type");
		    DebugLogger.d("NotificationService", "-- command_type -- " + commandType);
		    if(!TextUtils.isEmpty(commandType) && commandType.equals("reflect_receiver")) {
			this.reflectReceiver(intent);
		    }
		}

	    }

	    @Override
	    public boolean onUnbind(Intent intent) {
		return super.onUnbind(intent);
	    }

	    public String getReceiver(String var1, String var2) {
		if(!TextUtils.isEmpty(var1) && !TextUtils.isEmpty(var2)) {
		    String var3 = null;
		    List var4;
		    Intent var5 = new Intent(var2);
		    var5.setPackage(var1);
		    PackageManager var6 = this.getPackageManager();
		    var4 = var6.queryBroadcastReceivers(var5, 0);
		    if(var4 != null && var4.size() > 0) {
			var3 = ((ResolveInfo)var4.get(0)).activityInfo.name;
		    }
		    return var3;
		} else {
		    return null;
		}
	    }

	    public void reflectReceiver(Intent intent) {
		String receiver = this.getReceiver(this.getPackageName(), intent.getAction());
		if(TextUtils.isEmpty(receiver)) {
		    Log.i("NotificationService", " reflectReceiver error: receiver for: " + intent.getAction() + " not found, package: " + this.getPackageName());
		    intent.setPackage(this.getPackageName());
		    this.sendBroadcast(intent);
		} else {
		    try {
			Class receiverClass = Class.forName(receiver);
			Constructor receiverClassConstructor = receiverClass.getConstructor((Class[])null);
			Object newInstance = receiverClassConstructor.newInstance((Object[])null);
			Class[] var7 = new Class[]{Context.class, Intent.class};
			Method onReceive = receiverClass.getMethod("onReceive", var7);
			intent.setClassName(this.getPackageName(), receiver);
			Object[] params = new Object[]{this.getApplicationContext(), intent};
			onReceive.invoke(newInstance, params);
		    } catch (Exception var11) {
			Log.i("NotificationService", "reflect e: " + var11);
		    }

		}
	    }
	}


## 三. Push自定义二进制协议

### 3.1 Push消息定义

这里我们采用protobuf定义协议格式


	message NotifyBody
	{
	    required string app     = 1;
	    required string body = 2;
	    optional string ext = 3;  //扩展信息(json格式){"ct1":{"pushType":"0"}, "statics":{"taskId":"123456"}, ...}
	};

协议的定义保证了客户端与服务器的通信可靠性，为了保证推送的准确实时到达，对于每一条收到的推送消息，收到之后都要向push服务器报告消息接收成功

图以后补上

## 四. 长连接保持处理方案

### 4.1 准备工作

   在客户端连接服务端之前服务器的选择问题，断线重连问题

### 4.2 重定向

   请求push服务器地址列表，便于进行服务器选取
### 4.3 服务器优先级选择

   进行服务器的跑马选择，确定最优的服务器连接地址，可能有网络环境有关，包括数据网络类型：Wifi,4G
### 4.4 断线重连机制

   在服务器心跳连接过程中，可能出现网络中断，有必要设计网络重连机制。网络状态的变更必须促发新的连接请求
### 4.5 鉴权

   与服务器建立连接之后，开始进行鉴权，确认该设备的连接的合法性，只有鉴权成功后，应用才可能成功发起注册，并接收推送消息
### 4.6 心跳设计

   心跳的设计主要是保持通道的畅通性，以确保在推送消息到达后能够通过此通道顺利的到达手机端

#### 4.6.1 启动时机

  通道连接成功后，启动心跳连接
#### 4.6.2 心跳初始间隔策略

	mMin: 最小间隔
	mMax: 最大间隔
	mInternal: 初始心跳间隔 = mMin + (mMax-mMin)>>3

#### 4.6.3 心跳间隔步进步减策略

  * 步减条件

  超时回应：规定时间内没有收到ping回应消息
  心跳发送失败：无法读取服务的ping回应消息，暂停发送心跳

	mMin = 180 (重置)
	mMax = mInternal
	mInternal -= (mMax-mMin)>>>2

  * 步增条件

  心跳发送成功：timesOK++

	++mTimesOk >= mInterval / 100 * 2
	timesOK: 心跳发送成功数量
	mMin = mInternal
	mInternal =+ (mMax-mMin)>>2

*NOTE: 以下是步增算法示例代码*

	public void print(){
			int min = 180;
			int max = 890;
			int mInternal = 180+((max-min)>>3);
			System.out.println("max="+max+" min="+min+" mInternal="+mInternal);
			while(max>mInternal){
				int step = (max-min)>>3;
				min = mInternal;
				mInternal += step;
				System.out.println("step="+step+" max="+max+" min="+min+" mInternal="+mInternal+" minute = "+mInternal/60+"分");
				if(step == 0){
					break;
				}
			}
		}

如果中间不发生中断运行间隔时间结果如下(精确到分钟)

	step=88 max=890 min=268 mInternal=356 minute = 5分
	step=77 max=890 min=356 mInternal=433 minute = 7分
	step=66 max=890 min=433 mInternal=499 minute = 8分
	step=57 max=890 min=499 mInternal=556 minute = 9分
	step=48 max=890 min=556 mInternal=604 minute = 10分
	step=41 max=890 min=604 mInternal=645 minute = 10分
	step=35 max=890 min=645 mInternal=680 minute = 11分
	step=30 max=890 min=680 mInternal=710 minute = 11分
	step=26 max=890 min=710 mInternal=736 minute = 12分
	step=22 max=890 min=736 mInternal=758 minute = 12分
	step=19 max=890 min=758 mInternal=777 minute = 12分
	step=16 max=890 min=777 mInternal=793 minute = 13分
	step=14 max=890 min=793 mInternal=807 minute = 13分
	step=12 max=890 min=807 mInternal=819 minute = 13分
	step=10 max=890 min=819 mInternal=829 minute = 13分
	step=8 max=890 min=829 mInternal=837 minute = 13分
	step=7 max=890 min=837 mInternal=844 minute = 14分
	step=6 max=890 min=844 mInternal=850 minute = 14分
	step=5 max=890 min=850 mInternal=855 minute = 14分
	step=5 max=890 min=855 mInternal=860 minute = 14分
	step=4 max=890 min=860 mInternal=864 minute = 14分
	step=3 max=890 min=864 mInternal=867 minute = 14分
	step=3 max=890 min=867 mInternal=870 minute = 14分
	step=2 max=890 min=870 mInternal=872 minute = 14分
	step=2 max=890 min=872 mInternal=874 minute = 14分
	step=2 max=890 min=874 mInternal=876 minute = 14分
	step=2 max=890 min=876 mInternal=878 minute = 14分
	step=1 max=890 min=878 mInternal=879 minute = 14分
	step=1 max=890 min=879 mInternal=880 minute = 14分
	step=1 max=890 min=880 mInternal=881 minute = 14分
	step=1 max=890 min=881 mInternal=882 minute = 14分
	step=1 max=890 min=882 mInternal=883 minute = 14分
	step=1 max=890 min=883 mInternal=884 minute = 14分
	step=0 max=890 min=884 mInternal=884 minute = 14分



#### 4.6.4 夜间模式

适当延长心跳时间间隔


## 问题汇总
