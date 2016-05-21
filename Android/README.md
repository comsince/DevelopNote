---
layout: post
title: "Android 技能汇总"
description: 这里主要整理android开发从初级入门到逐步深入的资料，包含网上分享的资料，技术博客。这里整理的包括自己以及自己觉得不错的文档。技术学习到此，可能有一些瓶颈的时候，更应该分享一下技术，整理之前学习的历程，同时伴随着技术的不断发展，也会不断涌现一些最新的技术，更需要我们不断的学习深入。
category: blog
---


## 概述
这里主要整理android开发从初级入门到逐步深入的资料，包含网上分享的资料，技术博客。这里整理的包括自己以及自己觉得不错的文档。技术学习到此，可能有一些瓶颈的时候，更应该分享一下技术，整理之前学习的历程，同时伴随着技术的不断发展，也会不断涌现一些最新的技术，更需要我们不断的学习深入。

* [如何自学Android编程 ](http://stormzhang.com/android/2016/01/21/learn-android-byself/#rd?sukey=16298ae1a3e33631d8ff97a89eec05d671fc1dcc6cce14e4aaa88d5b3ea7159b69c06477975258e0a9c46d6dee424b4e)


## Android日志分析工具
* [日志打印](http://blog.csdn.net/hansel/article/details/38088583)

## 源码技术解析
知名的开源项目的源码技术，帮你在开发过程中灵活使用和做出有效的扩展，并且帮助你提升设计能力

* [EventBus 源码解析](http://a.codekk.com/detail/Android/Trinea/EventBus%20%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90)
* [Volley 源码解析](http://a.codekk.com/detail/Android/grumoon/Volley%20%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90)

## Http解析
* [Http协议说明](http://kb.cnblogs.com/page/130970/#whathttp)
   主要说明http协议的组成部分，方便分析okhttp以及HttpClient的源码

## Http模拟请求工具
* [HttpTest](http://www.atool.org/httptest.php)

* [Volley http demo](https://github.com/smanikandan14/Volley-demo)

## Push推送实践
推送技术在Android的实现中一直多种变化的，归根结底都是需要客户端需要与服务端建立一种长连接，从而在需要推送消息时能够保持通道的畅通性

* [第三方PushSDK 设计实践](../push/push_design_thirdparty_doc.md)

关于长连接实现方案，目前开源的方案大多采用netty等开源项目,当然也可以基于NIO实现长连接

* [Android 保持存活率问题](http://www.oschina.net/news/72685/android-process)

## 协议定义Protobuf
* [Google ProtoBuf](https://developers.google.com/protocol-buffers/docs/javatutorial#compiling-your-protocol-buffers)
首先是写出proto文件，利用protobuf的自带的java生成工具生成java文件即可

## 代码混淆
### aar包发布时混淆代码问题
这里的混淆文件是编译aar包时的混淆文件

### aar包发布时将混淆文件同时发布
这里的混淆文件是随aar包发布，需要用户apk配置的混淆文件


## 编译工具
### Gradle
#### Gradle 系列课程
* [魅族grdle培训课程](http://www.slideshare.net/JweenLau/)

#### Dependencies for different build types
主要说明在不同编译类型下，如何动态配置依赖库，使其在不同的buildType下使用不同的依赖库
* (1)[Provide configurations for flavor+type combinations](https://code.google.com/p/android/issues/detail?id=162285)
* (2)[Gradle dependency based on both build type and flavor](http://stackoverflow.com/questions/28137853/gradle-dependency-based-on-both-build-type-and-flavor)

#### [Write Custom Gradle Plugin](./2016-05-14-write-custom-gradle-plugin.md)

## Android API

### Android 相关教程

* [Android官方培训课程中文版(v0.9.5)](http://hukai.me/android-training-course-in-chinese/index.html)

### Android Notification
