### Q.Error:Configuration with name 'default' not found.
> A.这种问题一般情况是由于该project内子工程无法找到自己的资源
> 如下：
> include ':Common'
> project(':Common').projectDir = new File(settingsDir, '../Media/Common')

> 如果settingDir指定的目录没有该项目的源文件就会出现上诉的错误
