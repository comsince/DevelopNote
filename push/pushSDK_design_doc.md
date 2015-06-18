#PushSDK设计文档
## 一 设计需求
  pushSDK 为了能在一台手机中维持单通道的设计，必须立足利用SDK维单Service;单service在系统层面上控制手机系统只会存在一个推送服务，这个全局推送服务确保了单通道的可能性
  
## 二 技术问题
 
  * (1) 如何维持系统中单servcie
     **Q: ** 在Android 5.0 以下版本，如果设置全部进行，无论系统中多次调用service，系统只会保留第一个启动它的实例，这样就保证了系统中维持一个全局pushServcie的可能，但是由于在Android 5.0 及以后版本，系统对调用隐式调用service做了限制，必须显式执行该service的所在包名，由于系统中接入pushSDK的应用众多，如果不加限制，将会出现无法决定改启用那个包名下的pushservcie，如果任由随意创建，系统将会出现多个pushservice 
     **A:** 设计启动pushservice的选取策略，参考现有市面上的推送SDK，原则上这个包名的选择是，选取第一个启动该pushservice的包名，只要系统没有重启，以后不管哪个集成pushSDK的应用，首次启动的包名都将是这个指定的包名
> **NOTE:**　鉴于在Flyme系统上CloudService作为系统应用内置于其中，为了保证pushService存活度，现在默认将启动pushSercie的包名设置为云服务的包名，当前前提是cloudservic集成了pushSDK；基于Flyme系统，随后可能系统中的应用将会逐步接入PushSDK，这样系统应用将会抱团，也就是说即使pushService由于某些原因终止运行，系统中任何一个集成pushSDK的应用由于用户点击启动，都将会带起pushService

  * (2) 跨进程启动应用问题
    **Q: ** Notification 在设置PendingIntent时由于存在跨应用跳转会报出安全权限错误
  * (3) 三方应用消息接收度
    **Q: ** 由于PushSDK收到消息后是通过广播发送出去的，如果第三方应用由于某种原因并没有在系统中运行，因此其就不能收到pushSDK发送的广播消息，这样消息的送达率会很低
    **A: ** 经过对现在市面上的pushSDK的分析得出，要想使第三方应用收到消息，首先的保证第三方应用正在运行，因此我们需要加一个notificationService,当通知弹出时，需要启动要发送的包名的下的notificationService，这样在我们点击消息后，在去发送通知广播，第三方应用就会在自己的receiver处理比如打开应用，打开应用内页面，也就不会出现跨应用的问题
