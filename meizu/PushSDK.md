##魅族云推送集成说明

* 1.在自定义Application的onCreate方法中调用Push绑定接口
  
```java
   PushManager.register(Context context,String appId)
```

* 2.AndroidManifest.xml声明permission

```xml
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
```

* 3.AndroidManifest.xml注册消息接收receiver
    客户端需要自己实现MyPushMessageReceiver，接收Push服务的消息，并实现对消息的处理

    MyPushMessageReceiver如下代码所示，继承com.meizu.cloud.pushsdk.MzPushMessageReceiver

```java
    public class MyPushMsgReceiver extends MzPushMessageReceiver {
    @Override
    public void onRegister(Context context, String s) {

    }

    @Override
    public void onMessage(Context context, String s) {

    }
}
```

```xml
<!-- push应用定义消息receiver声明 -->
        <receiver android:name="your.package.MyPushMsgReceiver">
            <intent-filter>
                <!-- 接收push消息 -->
                <action android:name="com.meizu.cloud.pushservice.action.ON_MESSAGE" />
            </intent-filter>
        </receiver>
```

* 4.AndroidManifest.xml增加pushservice配置

```xml
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
```

## 推送服务问题说明
### MzPushService 配置相关说明
* 1 独立应用MzPushService 配置process不同
> 1.1 第一类配置：android:process="com.meizu.cloud.push"
```
        <service
            android:name="com.meizu.cloud.pushsdk.pushservice.MzPushService"
            android:exported="true"
            android:process="com.meizu.cloud.push" >
            <intent-filter >
                <action android:name="com.meizu.cloud.pushservice.action.PUSH_SERVICE"/>
            </intent-filter>
        </service>
```
> 1.2 第二类配置：android:process="com.meizu.cloud.pushservice"
```
        <service
            android:name="com.meizu.cloud.pushsdk.pushservice.MzPushService"
            android:exported="true"
            android:process="com.meizu.cloud.pushservice" >
            <intent-filter >
                <action android:name="com.meizu.cloud.pushservice.action.PUSH_SERVICE"/>
            </intent-filter>
        </service>

```

> 1.3 配置私有进程名
> 可能出现的问题
> 1.如果多个应用同时配置全局process，并且名称不一样，系统以第一个启动的应用配置的process为准
   实践证明，无论是全局配置，还是私有配置process，系统只会有一个MzPushService

* 2.配置pushServiceReceiver问题
```
        <receiver android:name="com.meizu.cloud.pushsdk.PushServiceReceiver"
                  android:permission="android.intent.action.BOOT_COMPLETED"
                  android:process="com.meizu.cloud.push">
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

```

> 配置receiver 的process要与MzPushService配置的process保持一致
> android:process="com.meizu.cloud.push"
> 如果该receiver与MzPushService运行在不用的进程中，应用收到通知就会启动MzPushService，从而使系统出现多个MzPushService


