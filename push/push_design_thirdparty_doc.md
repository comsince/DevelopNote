# ������PushSDK ���ʵ��
# һ. ����
   * �������͵ĵ�����ʵ���ɺܶ࣬�ٶ������ͣ��������ͣ����ƣ��������ͣ�֮ǰ�Ѿ����⼸�����͵ľ���ԭ��ͽ��뷽ʽ�й�˵�����������[��ƽ̨���ͽ������]()
   * ���ͱ��ʾ�����push���񱣳�һ����Ч�ĳ����ӣ�����ʵʱ����Ϣ���͵��ֻ��ͻ��ˣ�����Ҳ��������ʵ�ֵĸ�������������������з�����
   * ���ڸ��������о�������ص���Ŀ��������Ծ����ʵ����һЩ��Ҫ�ķ���

# ��. ����Ӧ�ô��������
��������Ҫ����SDK���������У�AndroidӦ�ü����е�����
## 2.1 SDK��������
SDK������Ҫ�ø���Ӧ���Դ��뼯�ɵķ�ʽ���룬�ڴ�ͳ�Ŀ���ģʽ�У�Ӧ��Ҫ���������SDK����Ȼ������һЩĬ�ϵ����ã�����AndroidManifest.xml�����ã�����
```
<receiver android:name="com.meizu.cloud.PushMsgReceiver">
            <intent-filter>
                <!-- ����push��Ϣ -->
                <action android:name="com.meizu.flyme.push.intent.MESSAGE" />
                <!-- ����register��Ϣ-->
                <action android:name="com.meizu.flyme.push.intent.REGISTER.FEEDBACK"/>
                <!-- ����unregister��Ϣ-->
                <action android:name="com.meizu.flyme.push.intent.UNREGISTER.FEEDBACK"/>
                <action android:name="com.meizu.c2dm.intent.REGISTRATION" />
                <action android:name="com.meizu.c2dm.intent.RECEIVE" />
                <category android:name="com.meizu.pushdemo"></category>
            </intent-filter>

        </receiver>
```

�������ϵ������⣬SDK�����п��ܰ���������so��ļ��ɣ���Щ����Ҫ�û��ֶ����룬�������ϸ�������Ӧ�ô����������ŵļ��ɷ�ʽ������ѡ��aar�ļ��ɷ�ʽ

* �������
 Ŀǰ����ȫ������Gradle�ı��뷽ʽ�����е���ⶼ����Android Studio library���̵�Ŀ¼�ṹ������֯���������ĺô�������Android����Ӧ���������so�⣬AndroidManifest���ɣ�proguard�������⣬Ӧ��ֻ��ҪӦ��aar���Ϳ��ԣ�ͬʱaar��Ҳ���Է�����artifactory�ϣ�����Ӧ�þͿ���������ķ�ʽ���ü��ɣ�

```
    compile 'com.meizu.cloud.pushsdk:open:3.0.2-beta@aar'
    proguard 'com.meizu.cloud.pushsdk:open:3.0.2-beta@pro'
    compile 'com.meizu.cloud.pushsdk:internal:3.0.3-beta@aar'
    proguard 'com.meizu.cloud.pushsdk:internal:3.0.3-beta@pro'
```


## 2.1 SDK�����̵�Serviceʵ��
### 2.1.1 ͨ�����ӵĳ־���
  SDK��Ҫ��push���񱣳ֳ����ӣ�һ���ձ�������ǲ���Android Service�ķ�ʽ��פ���������ܱ�֤SDK��push���񱣳�һ�ֳ��õ�����
### 2.1.2 ͨ����Ψһ�Ա�֤
  ���ڶ��ֻ������Ŀ����Լ�push�������ĳ���������һ�����PushServiceһ����һ̨�ֻ���ֻ��Ĭ�Ϲ��ص�һ��Ӧ���ϣ�ֻ���������ܱ���Service��Ψһ��

```
Intent pushServiceIntent = new Intent(PushConstants.MZ_PUSH_ON_START_PUSH_REGISTER);
pushServiceIntent.setClassName(context.getPackageName(),"com.meizu.cloud.pushsdk.pushservice.MzPushService");
pushServiceIntent.putExtra(PushConstants.REGISTER_PACKAGE_NAME,context.getPackageName());
context.startService(pushServiceIntent);
```
 
### 2.1.3 PushService����ѡ��
����������push�����Ψһ�ԣ�Ψһ�Ա�Ȼ����������Ǹ�PushService��������һ�������£���Flyme���ɵ��£����service�����Ĭ�����Ʒ�����أ�������Flyme�����н����push��Ӧ�õ���Ϣ��������Service�յ�ת����Ϣ

### 2.1.4 ������ROM��������
  PushSDK���ڻ�Ҫ���������ROM���ͱ�Ȼ���ٵ�����SDK�����ٵ����⣬�����м���pushSDK��Ӧ��ͬʱ������һ���ֻ��ϣ����Ǳ�Ȼ��һ�����ȼ��㷨��PushService���׹������ĸ�Ӧ��֮��

* �������
���ǲ��õĳ��������Ǹ���Ӧ�õİ�����˳�����ȼ���ߵ�Ӧ��������Ӧ���µ�PushService����ȻΪ�˷�ֹ�����α��service�����ǻ�������PushService֮����໥У�����

## 2.2 Ӧ����Ϣ������
������Ϣ�����ʣ���֤��Ϣ׼ȷ��Ӧ��
* ���ʹ��㲥
  ͨ���㲥������Ϣ�п���Ӧ���˳�֮���޷��յ��㲥��Ϣ
* ͨ��Serviceת����Ϣ
  Ϊ�˽�������Ĺ㲥ת����Ϣ���������һ��ͨ��Serviceת����Ϣ�İ취�������취�ڰٶ���������Ҳʹ�ù��������������£�

```
  ͨ������NotificationService->����Receiver��OnReceive����
```
  ��ʱ���Է�ʽ���ַ�ʽ�����쳣�����Խ�Ϲ㲥�ķ�ʽ��ͬʹ�ã���֤��Ϣ������

������NotificationServcie�Ĵ���:
```
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

```
# ��. Push�Զ��������Э��
## 3.1 Push��Ϣ����
�������ǲ���protobuf����Э���ʽ

```
message NotifyBody
{
    required string app     = 1;
    required string body = 2;
    optional string ext = 3;  //��չ��Ϣ(json��ʽ){"ct1":{"pushType":"0"}, "statics":{"taskId":"123456"}, ...}
};
```
Э��Ķ��屣֤�˿ͻ������������ͨ�ſɿ��ԣ�Ϊ�˱�֤���͵�׼ȷʵʱ�������ÿһ���յ���������Ϣ���յ�֮��Ҫ��push������������Ϣ���ճɹ�

ͼ�Ժ���

# ��. �����ӱ��ִ�����
## 4.1 ׼������
   �ڿͻ������ӷ����֮ǰ��������ѡ�����⣬������������
## 4.2 �ض���
   ����push��������ַ�б����ڽ��з�����ѡȡ
## 4.3 ���������ȼ�ѡ��
   ���з�����������ѡ��ȷ�����ŵķ��������ӵ�ַ�����������绷���йأ����������������ͣ�Wifi,4G
## 4.4 ������������
   �ڷ������������ӹ����У����ܳ��������жϣ��б�Ҫ��������������ơ�����״̬�ı������ٷ��µ���������
## 4.5 ��Ȩ
   ���������������֮�󣬿�ʼ���м�Ȩ��ȷ�ϸ��豸�����ӵĺϷ��ԣ�ֻ�м�Ȩ�ɹ���Ӧ�òſ��ܳɹ�����ע�ᣬ������������Ϣ
## 4.6 �������
   �����������Ҫ�Ǳ���ͨ���ĳ�ͨ�ԣ���ȷ����������Ϣ������ܹ�ͨ����ͨ��˳���ĵ����ֻ���

### 4.6.1 ����ʱ��
  ͨ�����ӳɹ���������������
### 4.6.2 ������ʼ�������
  mMin: ��С���
  mMax: �����
  mInternal: ��ʼ������� = mMin + (mMax-mMin)>>3
### 4.6.3 �������������������

  * ��������
  ��ʱ��Ӧ���涨ʱ����û���յ�ping��Ӧ��Ϣ
  ��������ʧ�ܣ��޷���ȡ�����ping��Ӧ��Ϣ����ͣ��������
```
  mMin = 180 (����)
  mMax = mInternal
  mInternal -= (mMax-mMin)>>>2
```
  
  * ������������

  �������ͳɹ���timesOK++
++mTimesOk >= mInterval / 100 * 2
  timesOK: �������ͳɹ�����
```
  mMin = mInternal
  mInternal =+ (mMax-mMin)>>2
```
*NOTE: �����ǲ����㷨ʾ������*
```
public void print(){
		int min = 180;
		int max = 890;
		int mInternal = 180+((max-min)>>3);
		System.out.println("max="+max+" min="+min+" mInternal="+mInternal);
		while(max>mInternal){
			int step = (max-min)>>3;
			min = mInternal;
			mInternal += step;
			System.out.println("step="+step+" max="+max+" min="+min+" mInternal="+mInternal+" minute = "+mInternal/60+"��");
			if(step == 0){
				break;
			}
		}
	}
```
����м䲻�����ж����м��ʱ��������(��ȷ������)
```
step=88 max=890 min=268 mInternal=356 minute = 5��
step=77 max=890 min=356 mInternal=433 minute = 7��
step=66 max=890 min=433 mInternal=499 minute = 8��
step=57 max=890 min=499 mInternal=556 minute = 9��
step=48 max=890 min=556 mInternal=604 minute = 10��
step=41 max=890 min=604 mInternal=645 minute = 10��
step=35 max=890 min=645 mInternal=680 minute = 11��
step=30 max=890 min=680 mInternal=710 minute = 11��
step=26 max=890 min=710 mInternal=736 minute = 12��
step=22 max=890 min=736 mInternal=758 minute = 12��
step=19 max=890 min=758 mInternal=777 minute = 12��
step=16 max=890 min=777 mInternal=793 minute = 13��
step=14 max=890 min=793 mInternal=807 minute = 13��
step=12 max=890 min=807 mInternal=819 minute = 13��
step=10 max=890 min=819 mInternal=829 minute = 13��
step=8 max=890 min=829 mInternal=837 minute = 13��
step=7 max=890 min=837 mInternal=844 minute = 14��
step=6 max=890 min=844 mInternal=850 minute = 14��
step=5 max=890 min=850 mInternal=855 minute = 14��
step=5 max=890 min=855 mInternal=860 minute = 14��
step=4 max=890 min=860 mInternal=864 minute = 14��
step=3 max=890 min=864 mInternal=867 minute = 14��
step=3 max=890 min=867 mInternal=870 minute = 14��
step=2 max=890 min=870 mInternal=872 minute = 14��
step=2 max=890 min=872 mInternal=874 minute = 14��
step=2 max=890 min=874 mInternal=876 minute = 14��
step=2 max=890 min=876 mInternal=878 minute = 14��
step=1 max=890 min=878 mInternal=879 minute = 14��
step=1 max=890 min=879 mInternal=880 minute = 14��
step=1 max=890 min=880 mInternal=881 minute = 14��
step=1 max=890 min=881 mInternal=882 minute = 14��
step=1 max=890 min=882 mInternal=883 minute = 14��
step=1 max=890 min=883 mInternal=884 minute = 14��
step=0 max=890 min=884 mInternal=884 minute = 14��
```


### 4.6.4 ҹ��ģʽ
�ʵ��ӳ�����ʱ����


# �������
