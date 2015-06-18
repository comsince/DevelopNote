在开发中遇到了一个问题，
在应用任意界面时，不仅仅是传递应用的activity名称 ，而是在AndroidManifest中声明的名称；
比如有一个activity在声明如下：
<activity android:name=".ui.MMPublishActivity"
                   android:screenOrientation="portrait"
                   android:alwaysRetainTaskState="true"
                  >
</activity>

push平台需要传递的activity如下：
.ui.MMPublishActivity
其中的字符全部包括一个都不能少！


/*try {
                Context packageContext = context.createPackageContext(pacakageName,Context.CONTEXT_INCLUDE_CODE | Context.CONTEXT_IGNORE_SECURITY);
                Log.i(TAG,"packageContext "+packageContext.getPackageName());
                pushNM = new PushNotificationManager(packageContext);
                pushNotificationManagerMap.put(pacakageName,pushNM);
            } catch (PackageManager.NameNotFoundException e) {
                e.printStackTrace();
            }*/




# PushSDK 设计问题
* 1 android L 同一应用无法发送不同进程的广播
* 2 单service设计思想
    android 4.4及一下版本service设置process，不同应用隐式调用service，系统只会出现一个service进程，但是在android5.0上会出现多个进程
* 3.多进程启动通知调用其他应用Activity的问题
    PendingIntent在设置启动activity时，只能是本context涉及的Acitvity（既是本包下的Activity），不然点击通知栏并不会跳转到其指定的Activity

* 4.通知栏弹出方式
    发送跨应用通知栏

* 5.绑定service是否会出现多个service进程
* 6.跨应用吊起Activity
     由于跨应用吊起Activity存在权限问题，需要将消息发送给receiver，再行处理；
* 7.启动pushService的首应用选择

