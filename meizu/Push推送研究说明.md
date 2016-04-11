---
layout: post
title: "Push推送对比研究分析"
description: 针对市场各个推送SDK做一些简要的说明，方便SDK的设计与开发
category: blog
---


# Push推送对比研究分析
[TOC]
## 个推
### 一.主要功能及特点

 1. 个推不仅能提供云端到客户端的推送服务，也可以提供从客户端上传至云端的服务，即推送消息链路支持上下行双向通道，开发者与客户端之间互动更便利。
 2. 多个APP合并一条长连接，共享链路，省电省流量。
 3. SDK接口丰富，可定制推送模式和通知栏提示样式，也支持增量更新。
 4. 通过根据用户属性的分析建立不同标签，也可以进行A/B分组测试，从而进行精细化运营。
 5. 保持与服务器的长连接，以便消息能够即时推送到达客户端 

### 二.Android SDK推送框架
   个推 Android SDK 是作为 Android Serivice 长期运行在后台的，从而创建并保持长连接，保持永远在线的能力。当开发者想要及时地推送消息到达 App 时，只需要调用个推 API推送，即可轻松与用户交流。

如下图所示，为个推推送服务框架图：

![enter image description here](http://docs.igetui.com/download/attachments/1214581/image2015-4-21+17:27:28.png?version=1&modificationDate=1429609417000)
### 三.集成说明

   集成主要包括【资源集成】,【AndroidManifest.xm配置】，【项目代码初始化】
   

####3.1 资源集成
  此部分集成主要是将第三方提供的SDK中的jar以及so库文件集成到自己项目中，此处可以参见
     [个推资源集成说明](http://docs.igetui.com/pages/viewpage.action?pageId=589991#AndroidSDK%E9%9B%86%E6%88%90%E6%AD%A5%E9%AA%A4-1%E5%AF%BC%E5%85%A5%E4%B8%AA%E6%8E%A8SDKLIB)
     
#### 3.2 AndroidManifest.xm配置
##### 3.2.1 权限声明

	<uses-permissionandroid:name="android.permission.INTERNET"/>
	<uses-permissionandroid:name="android.permission.READ_PHONE_STATE"/>
	<uses-permissionandroid:name="android.permission.ACCESS_NETWORK_STATE"/>
	<uses-permissionandroid:name="android.permission.CHANGE_WIFI_STATE"/>
	<uses-permissionandroid:name="android.permission.WAKE_LOCK"/>
	<uses-permissionandroid:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
	<uses-permissionandroid:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
	<uses-permissionandroid:name="android.permission.VIBRATE"/>
	<uses-permissionandroid:name="android.permission.ACCESS_WIFI_STATE"/>
	<uses-permissionandroid:name="getui.permission.GetuiService.第三方包名"/>
	<uses-permissionandroid:name="android.permission.GET_TASKS"/>
	<!--自定义权限-->
	<permission
	    android:name="getui.permission.GetuiService.第三方包名"
	    android:protectionLevel="normal">
	</permission>

##### 3.2.2 服务项配置 

	<!--个推SDK配置开始-->
	<!--配置第三方应用参数属性-->
	<meta-data
	    android:name="PUSH_APPID"
	    android:value="你的APPID"/><!--替换为第三方应用的APPID-->
	<meta-data
	    android:name="PUSH_APPKEY"
	    android:value="你的APPKEY"/><!--替换为第三方应用的APPKEY-->
	<meta-data
	    android:name="PUSH_APPSECRET"
	    android:value="你的APPSECRET"/><!--替换为第三方应用的APPSECRET-->
	<meta-data
	    android:name="PUSH_GROUPID"
	    android:value=""/>
	<!--配置SDK核心服务-->
	<service
	    android:name="com.igexin.sdk.PushService"
	    android:exported="true"
	    android:label="NotificationCenter"
	    android:process=":pushservice">
	</service>
	<receiver 
	    android:name="com.igexin.sdk.PushReceiver">
	    <intent-filter>
		<action android:name="android.intent.action.BOOT_COMPLETED"/>
		<action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>
		<action android:name="android.intent.action.USER_PRESENT"/>
		<action android:name="com.igexin.sdk.action.refreshls"/>
	    </intent-filter>
	</receiver>
	<receiver
		android:name="com.igexin.sdk.PushManagerReceiver"
		android:exported="false" >
		<intent-filter>
		    <action android:name="com.igexin.sdk.action.pushmanager" />
		</intent-filter>
	</receiver>
	<activity
	    android:name="com.igexin.sdk.PushActivity"
	    android:excludeFromRecents="true"
	    android:exported="false"
	    android:process=":pushservice"
	    android:taskAffinity="com.igexin.sdk.PushActivityTask"
	    android:theme="@android:style/Theme.Translucent.NoTitleBar">
	</activity>
	<!--配置弹框activity-->
	<activity 
	    android:name="com.igexin.getuiext.activity.GetuiExtActivity"
	    android:configChanges="orientation|keyboard|keyboardHidden"
	    android:excludeFromRecents="true"
	    android:process=":pushservice"
	    android:taskAffinity="android.task.myServicetask"
	    android:theme="@android:style/Theme.Translucent.NoTitleBar"
	    android:exported="false"/>
	<receiver 
	    android:name="com.igexin.getuiext.service.PayloadReceiver"
	    android:exported="false">
	    <intent-filter>
		<!--这个com.igexin.sdk.action.7fjUl2Z3LH6xYy7NQK4ni4固定，不能修改-->
		<action android:name="com.igexin.sdk.action.7fjUl2Z3LH6xYy7NQK4ni4"/>
		<!--替换为android:name="com.igexin.sdk.action.第三方的appId"-->
		<action android:name="com.igexin.sdk.action.你的APPID"/>
	    </intent-filter>
	</receiver>
	<service 
	    android:name="com.igexin.getuiext.service.GetuiExtService"
	    android:process=":pushservice"/>
	<!--个推download模块配置-->
	<service 
	    android:name="com.igexin.download.DownloadService"
	    android:process=":pushservice"/>
	<receiver
		android:name="com.igexin.download.DownloadReceiver">
		<intent-filter>
		    <action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>
		</intent-filter>
	</receiver>
	<provider
		 android:name="com.igexin.download.DownloadProvider"
		 android:process=":pushservice"
		 android:authorities="downloads.你的包名"/><!--替换为downloads.第三方包名-->

#### 3.3 项目代码初始化    
在您应用程序启动初始化阶段，初始化SDK

	PushManager.getInstance().initialize(this.getApplicationContext());

> **Note:**
> 该方法必须在Activity或Service类内调用。一般情况下，可以在Activity的onCreate()方法中调用。由于应用每启动一个新的进程，就会调用一次Application的oncreat方法，而个推SDK是一个独立的进程，会导致在一个应用中至少调用2次Application的onCreate()，这样会影响应用的处理逻辑，所以不建议在Application继承类中调用。
为保证意外情况导致初始化失败，建议应用程序每次启动时都调用一次该初始化接口。


## 百度云推送
### 一.主要功能及特点

 1. 推送通知 
 2. 推送消息
 3. 推送富媒体
 4. 基于地理位置的推送（或“LBS推送”）

### 二.Android SDK推送框架
 云推送的Android SDK，是通过后台service和socket长连接机制来实现的。从消息时效性、耗电量、网络流量等因素考虑，这是目前最好的实现方式。
在同一台设备安装了多个使用推送的应用的情况下，如果每个应用都执行独立的后台service，且各自建立独立的长连接，这无疑是系统资源的巨大浪费。内存使用、耗电量、网络流量等关键因素都将以接近与应用数正比的倍数增长。
在这个背景下，云推送实现了单服务单通道的机制。同一台设备上，云推送服务的资源消耗不受集成该服务的应用数量影响。任何时刻，只会运行一个后台service和维持一个socket长连接。
应用的初始化、tag等接口调用，将通过intent方式发送到后台运行的service处理。service接收到推送消息时，将根据消息中指定的发送对象，通过intent，以指定目标应用包名的方式，发送私有消息给应用。应用无法接收到不属于自己的消息，也无法通过冒充截获。
 如下图所示：
 
 ![enter image description here](http://developer.baidu.com/wiki/static/channel/image/scene-info.png)
  
### 三.集成说明
#### 3.1资源集成

> **Note:** [具体详见百度云推送SDK开发文档](http://push.baidu.com/doc/android/api)


#### 3.2 AndroidManifest.xm配置
##### 3.2.1 权限声明

	<!-- Push service 运行需要的权限 -->
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="android.permission.READ_PHONE_STATE" />
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />  
	<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
	<uses-permission android:name="android.permission.WRITE_SETTINGS" />
	<uses-permission android:name="android.permission.VIBRATE" />
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	<uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER"/>
	<uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
	<uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />

##### 3.2.2 服务项配置 

	<!-- push service start -->
	<!-- 用于接收系统消息以保证PushService正常运行 -->
	<receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
	    android:process=":bdservice_v1">
	    <intent-filter>
		<action android:name="android.intent.action.BOOT_COMPLETED" />
		<action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
		<action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
		<action android:name="com.baidu.android.pushservice.action.media.CLICK" />
		<!-- 以下四项为可选的action声明，可大大提高service存活率和消息到达速度 -->
		<action android:name="android.intent.action.MEDIA_MOUNTED" />
		<action android:name="android.intent.action.USER_PRESENT" />
		<action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
		<action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
	    </intent-filter>
	</receiver>
	<!-- Push服务接收客户端发送的各种请求-->
	<receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
	    android:process=":bdservice_v1">
	    <intent-filter>
		<action android:name="com.baidu.android.pushservice.action.METHOD" />
		<action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
	    </intent-filter>
	    <intent-filter>
		<action android:name="android.intent.action.PACKAGE_REMOVED"/>
		<data android:scheme="package" />
	    </intent-filter>                   
	</receiver>
	<service android:name="com.baidu.android.pushservice.PushService" android:exported="true" 
	    android:process=":bdservice_v1" >
	    <intent-filter >
		    <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE"/>
	    </intent-filter>
	</service>
	<!-- 4.4版本新增的CommandService声明，提升小米和魅族手机上的实际推送到达率 -->
	<service android:name="com.baidu.android.pushservice.CommandService"
	    android:exported="true" />

#### 3.3 项目代码初始化

 - 配置Application信息
 
 
	<application
	    android:name="com.baidu.frontia.FrontiaApplication"
	    android:icon="@drawable/ic_launcher"
	    android:label="@string/app_name">

 
 - 启动云推送


在当前工程的主Activity的onCreate函数中，添加以下代码：

	PushManager.startWork(getApplicationContext(),PushConstants.LOGIN_TYPE_API_KEY,"API KEY")


## 极光推送
### 一.主要功能及特点

 1. JPush Android SDK 是作为 Android Serivice 长期运行在后台的，从而创建并保持长连接，保持永远在线的能力。
 2. 多平台支持,JPush Android SDK 除了 jar 包，还有一个 .so 文件。.so 文件的存在 CPU 平台适配的问题。需要支持哪个平台的 CPU，就需要包含这个平台相应的 .so 编译文件。除支持默认的 ARM CPU 平台之外，JPush SDK 还提供 x86 与 MIPs 平台的 CPU 版本 SDK。请单独到 资源下载 页下载。
 3. 电量与流量,JPush Android SDK 由于使用自定义协议，协议体做得极致地小，流量消耗非常地小。电量方面，JPush Android SDK 经过持续地优化，尽可能减少不必要的代码执行；并且，长期的版本升级迭代，不断地调优，在保证一定的网络连接稳定性的要求小，减少电量消耗。

### 二.Android SDK推送框架
开发者集成 JPush Android SDK 到其应用里，JPush Android SDK 创建到 JPush Cloud 的长连接，为 App 提供永远在线的能力。
当开发者想要及时地推送消息到达 App 时，只需要调用 JPush API 推送，或者使用其他方便的智能推送工具，即可轻松与用户交流。
图中红色部分，是 JPush 与 App 开发者的接触点。手机客户端侧，App 需要集成 JPush SDK；服务器端部分，开发者调用 JPush REST API 来进行推送。
如下图：

![enter image description here](http://docs.jpush.io/client/image/jpush_android.png)
### 三.集成说明
#### 3.1资源集成

> **Note:**  [具体详见极光推送SDK包说明](http://docs.jpush.io/guideline/android_guide/)


#### 3.2 AndroidManifest.xm配置
##### 3.2.1 权限声明

    <!-- Required -->
    <permission android:name="Your Package.permission.JPUSH_MESSAGE" android:protectionLevel="signature" />

    <!-- Required -->
    <uses-permission android:name="You Package.permission.JPUSH_MESSAGE" />
    <uses-permission android:name="android.permission.RECEIVE_USER_PRESENT" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.WRITE_SETTINGS" /> <!--since 1.6.0 -->

    <!-- Optional. Required for location feature -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_LOCATION_EXTRA_COMMANDS" />
    <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />


##### 3.2.2 服务项配置 


        <!-- Required -->
        <service
            android:name="cn.jpush.android.service.PushService"
            android:enabled="true"
            android:exported="false" >
            <intent-filter>
                <action android:name="cn.jpush.android.intent.REGISTER" />
                <action android:name="cn.jpush.android.intent.REPORT" />
                <action android:name="cn.jpush.android.intent.PushService" />
                <action android:name="cn.jpush.android.intent.PUSH_TIME" />
            </intent-filter>
        </service>

        <!-- Required -->
        <receiver
            android:name="cn.jpush.android.service.PushReceiver"
            android:enabled="true" >
          <intent-filter android:priority="1000"> <!--since 1.3.5 -->
                <action android:name="cn.jpush.android.intent.NOTIFICATION_RECEIVED_PROXY" /> <!--since 1.3.5 -->
                <category android:name="Your Package" /> <!--since 1.3.5 -->
            </intent-filter> <!--since 1.3.5 -->
            <intent-filter>
                <action android:name="android.intent.action.USER_PRESENT" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_ADDED" />
                <action android:name="android.intent.action.PACKAGE_REMOVED" />
                <data android:scheme="package" />
            </intent-filter>
        </receiver>
     <!-- Required SDK核心功能-->
        <activity
            android:name="cn.jpush.android.ui.PushActivity"
            android:theme="@android:style/Theme.Translucent.NoTitleBar"
            android:configChanges="orientation|keyboardHidden" >
            <intent-filter>
                <action android:name="cn.jpush.android.ui.PushActivity" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="Your Package" />
            </intent-filter>
        </activity>
        <!-- Required SDK核心功能-->
        <service
            android:name="cn.jpush.android.service.DownloadService"
            android:enabled="true"
            android:exported="false" >
        </service>
        <!-- Required SDK核心功能-->
        <receiver android:name="cn.jpush.android.service.AlarmReceiver" />

        <!-- Required. For publish channel feature -->
        <!-- JPUSH_CHANNEL 是为了方便开发者统计APK分发渠道。-->
        <!-- 例如: -->
        <!-- 发到 Google Play 的APK可以设置为 google-play; -->
        <!-- 发到其他市场的 APK 可以设置为 xxx-market。 -->
        <!-- 目前这个渠道统计功能的报表还未开放。-->
        <meta-data android:name="JPUSH_CHANNEL" android:value="developer-default"/>
        <!-- Required. AppKey copied from Portal -->
        <meta-data android:name="JPUSH_APPKEY" android:value="Your AppKey"/> 

#### 3.3 项目代码初始化
init 初始化SDK

	public static void init(Context context)


可以设置调试模式

	// You can enable debug mode in developing state. You should close debug mode when release.
	public static void setDebugMode(boolean debugEnalbed)

## Flyme云推送
### 一.主要功能及特点
  app为了及时获取到服务器端的消息更新，一般会采用轮寻或者推送的方式来获取消息更新，轮寻导致终端设备流量、电量、等系统资源严重浪费，所以目前采用的比较广泛的是推送的方式，目前 Meizu 的 Push SDK 不能脱离 Flyme OS 存在，当该 SDK 脱离 Flyme OS 之后由于没有长链接导致不能正常收到推送消息。本 SDK 首先要解决的时长链接由 SDK 自己维护，同时还要解决的就是多个 app 引用同一个 SDK 时长链接的复用问题。
### 二.Android SDK推送框架
该 SDK 以 Android Service 方式运行，独占一个进程，该 Service 自己维护与推送服务器的长链接。如果一款手机安装了多个集成了 SDK 的手机应用，则只有一个 service 实例运行，不会每个应用都会开启一个后台 service，而是采用多个应用共享一个Push通道的方式，这就解决了长链接复用的问题，节省了对流量、电量的浪费。使用该 SDK 只需要关心 MzPushManager 提供的API，与 MzPushMessageReceiver 提供的回调接口以及相应的配置即可。
### 三.集成说明
#### 3.1资源集成
集成前准备工作
  PushSDk 提供一个Jar包以及两个.so文件，目录结构如下


	--libs
	  --armeabi
	  ---libcrypto_framwork.so
	  ---libsnappy-jni.so
	  --pushsdk-all-V1.0.jar

#### 3.2 AndroidManifest.xm配置
##### 3.2.1 权限声明

    <!-- Push service 运行需要的权限 -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <uses-permission android:name="android.permission.WRITE_SETTINGS" />
    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER"/>
    <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
    <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />

##### 3.2.2 服务项配置 
* (1)AndroidManifest.xml注册消息接收receiver


        <!-- push应用定义消息receiver声明 -->
        <receiver android:name="your.package.MyPushMsgReceiver">
            <intent-filter>
                <!-- 接收push消息 -->
                <action android:name="com.meizu.cloud.pushservice.action.ON_MESSAGE" />
            </intent-filter>
        </receiver>


* (2)客户端需要自己实现MyPushMessageReceiver，接收Push服务的消息，并实现对消息的处理
MyPushMessageReceiver如下代码所示，继承com.meizu.cloud.pushsdk.MzPushMessageReceiver

	public class MyPushMsgReceiver extends MzPushMessageReceiver {
	    @Override
	    public void onRegister(Context context, String s) {

	    }

	    @Override
	    public void onMessage(Context context, String s) {

	    }
	}

* (3)AndroidManifest.xml增加pushservice配置

        <service
            android:name="com.meizu.cloud.pushsdk.pushservice.MzPushService"
            android:exported="true"
            android:process="com.meizu.cloud.pushservice" >
            <intent-filter >
                <action android:name="com.meizu.cloud.pushservice.action.PUSH_SERVICE"/>
            </intent-filter>
        </service>
        
        用于接收系统消息，保证pushservice正常运行
        <receiver android:name="com.meizu.cloud.pushsdk.PushServiceReceiver"
                  android:process="com.meizu.cloud.pushservice">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <!-- 以下四项为可选的action声明，可大大提高service存活率和消息到达速度 -->
                <action android:name="android.intent.action.MEDIA_MOUNTED" />
                <action android:name="android.intent.action.USER_PRESENT" />
                <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
                <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.meizu.cloud.pushsdk.pushservice.PushBroadcastProcessorService"
            android:exported="true"/>

#### 3.3 项目代码初始化
* 在自定义Application的onCreate方法中调用Push绑定接口
  

   PushManager.register(Context context)


