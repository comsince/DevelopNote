### Q.Error:Configuration with name 'default' not found.
> A.这种问题一般情况是由于该project内子工程无法找到自己的资源
> 如下：
> include ':Common'
> project(':Common').projectDir = new File(settingsDir, '../Media/Common')

> 如果settingDir指定的目录没有该项目的源文件就会出现上诉的错误

### Q.Android Studio - How to increase Allocated Heap Size
    I looked at my Environment Variables and had a System Variable called _JAVA_OPTIONS with the value -Xms256m -Xmx512m, 
    after changing this to -Xms256m -Xmx1024m the max heap size increased accordingly. 
```
-Xms256m -Xmx1024m
```
    [原问题地址](http://stackoverflow.com/questions/18723755/android-studio-how-to-increase-allocated-heap-size)

