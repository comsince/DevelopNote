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
