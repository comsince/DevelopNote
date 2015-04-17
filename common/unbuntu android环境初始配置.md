##1.配置JDK
```
$sudo gedit /etc/profile
这里配置的系统环境变量
```
* 配置java environment
```
JAVA_HOME=/home/liujicheng/java/jdk1.6.0_12
export JRE_HOME=/home/liujicheng/java/jdk1.6.0_12/jre
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH

```

##2.配置Android SDK
在配置环境变量时如有不理解请参考如下的说明文档
--[环境变量配置说明](http://www.cnblogs.com/bluestorm/archive/2012/10/12/2721210.html)

```
sudo gedit ~/.profile
export PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin:/home/cmm/android-sdk-linux/tools:/home/cmm/android-sdk-linux/platform-tools
$ source ~/.profile


这是配置个人环境变量
android studio 出现无法找到android_home环境变量的问题解决方法：
配置android_home环境变量
```


##3.ADB 配置
*  64位系统，Ubuntu11.04，搭建JDK，Android环境，把android SDK复制过来后，里面的adb和其它命令的都不能使用。
错误提示：android-sdk-linux_86/platform-tools/adb: 没有那个文件或目录。
解决方案：由于是64bit的系统，而Android sdk只有32bit的程序，需要安装ia32-libs，才能使用。

* 运行如下命令：
```
# apt-get install ia32-libs
```


##4.魅族手机链接unbutu的问题
[具体详见下面的链接](http://jingyan.baidu.com/article/a3761b2ba329571576f9aa09.html)

adb 配置问题可参考内置光盘的内容，注意，在链接的时候一定要选择媒体设备MTP


##5.安装虚拟机
* 5.1 deb安装：
```
sudo dpkg -i package.deb
```


* 5.2 系统激活
* 5.3 系统全屏化
如果手动在设置中安装增强功能无效，可以在下面的虚拟光驱中选择VBoxGuestAdditions文件（注意系统是32位还是64位的）点击安装即可
* 5.4 共享目录设置

```
VBoxManage sharedfolder add "win-dev" -name "share" -hostpath "/home/liaojinlong/Share"
```

注意：执行此命令时，确保当前运行的虚拟机系统关闭
win 7 执行一下命令
```
net use z: \\vboxsvr\共享的目录名  这里就是share
```
如果显示命令执行成功，表示共享目录创建成功
