---
layout: post
title: "Jenkins+Gradle Plugin+Android Unit Test"
description: jenkin api and gradle plugin for Android unit test
category: blog
---

# 一. 概述
Android 自动化单元测试一直困扰着广大开发者，可以说是一种从入门到"放弃"的技术方案，本片文章将从下面几个方面说明我在利用自动化构建工具促进Android自动化测试的经验

* Jenkins自动化构建
* Gradle Plugin自动构建任务功能
* Android Unit Test 测试概述与说明

# 二. Jenkins
## 2.1 Jenkins 环境搭建
* [Jenkins Install](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins)
* [Securing Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Securing+Jenkins) 
  Jenkins 安全机制
  * [Basic auth 权限认证](https://wiki.jenkins-ci.org/display/JENKINS/Authenticating+scripted+clients)
## 2.2 jenkins job and plugin
### 2.2.1 [Remote access API](https://wiki.jenkins-ci.org/display/JENKINS/Remote+access+API)
  Remote API can be used to do things like these:
    * retrieve information from Jenkins for programmatic consumption.
    * trigger a new build
      ```
       curl -X POST http://username:token@jenkins.rnd.meizu.com/view/StandAlone/job/StandAlone_InternetPlatform_Common/build?token=common
      ```
    * create/copy jobs

### jenkins aritfactory plugin
* [Artifactory Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Artifactory+Plugin)
  jenkins 良好的扩展性，有很多相关的插件供开发者使用，artifactory plugin 可以集成artifactory自定发包的功能，这里我们主要使用artifactory插件的功能默认读取管理员账户的信息调用gradle插件自动发布aar包
# 三 Gradle 
## gradle plugin
   利用gradle plugin良好的扩展性，进行基本的流程规范，目前测试的基本流程如下：
   开发者上传代码->触发jenkin自动化测试任务->自动进行打包测试apk->调用ATS测试平台进行单元测试->上传测试结果->自动发布aar
## 2.1 利用gradle实现单元测试插件

# 四 Android Test Support Library
## 4.1
