---
layout: post
title: "博客内部说明-目录"
description: 个人Github博客内容说明,这里主要说明当前个人研究技术的归纳总结，此blog长期更新，置顶
category: blog
---

* [MarkDown 编辑器，推荐使用StackEdit](https://stackedit.io/editor)

## 关于github博客Markdown注意事项
* 代码高亮显示，注意引用代码必须加Tab
* ```*```必须注意空格行间隔

## 关于GitHub博客
  为了以后能够更好的发布博库，决定在github开通博客，使用[Jekyll](http://jekyllcn.com)
  在这里记录在安装jekyll的出现的问题，之所在这里说明，是想记录自己在windows上搭建jekyll博客所遇到的问题，之后再对详细的步骤做说明

  **NOTE:** 以下window安装过程中出现的问题，[Windows jekyll安装说明](http://jekyll-windows.juthilo.com)
  * [关于 ruby 安装devkit 的config.yml 文件](https://segmentfault.com/q/1010000003891132)

  这里问题有点坑，配置文件中，ruby的路径钱还要加一条横线。
  ruby gem 无法访问国外服务器，所以只能寻找过的镜像，淘宝的镜像用https证书问题，无法访问，所以就用了ruby-china的镜像

  * [ruby.org无法访问的问题](https://github.com/ruby-china/rubygems-mirror/issues/5)
  * [关于 Windows 下证书无法验证问题](https://github.com/ruby-china/rubygems-mirror/wiki)

## 概述
* 这个文档作为知识库的首要说明，主要记录开发经验积累，后续也会加上对一些具体案例的分析说明，加深对某一技术的理解这种模式应该成为一种学习的方式，遇到一个问题，对问题进行分类，类型维基百科的方法对改词条进行有效分类整理，并写出文档，形成一种知识的积累

## [Gihuber](http://githuber.cn/)
  这个网站对github上的相关注册账号的贡献进行了排名，可以看看自己的排名，项目的star,fork数直接影响排名

* [技术成长之路](http://comsince.github.io/person-improve)
  记录个人成长道路上的积累，以促进共勉，砥砺前行！

## 博客内容规划
对所有的新的技术进行系统的总结，从入门开始逐步深入，对各个过程的细节做出简要的分析；目前发现很多人在对新的技术适应能力不强，主要表现有些时候无从下手，不止从何开始，此内容主要就这一问题进行细致的讨论
目前准备对SDK设计的学习历程做一次系统性的总结；对于新技术的接收能力也是考验一个人适应未来的能力，但是在接收过程中对技术资料的归纳总结形成自己的经验，相信有很多人和我一样，存在一种迷糊的状态，并不能
很好的去形成自己的认知过程;大多数情况是我们缺少某一些基础知识导致对突如其来的技能显得有些措手不及，解决这个问题就是现在我要写博客的主要原因！
* 官方文档的逻辑梳理
  任何一个新的项目，当然这些流行的项目都会有详细的文档设计，文档的组织也就说明你的学习路线是什么，了解整个基础的过程对你不断渗入的理解打下基础
* 技术演进方式
  技术的演进本省需要自身的积累，需要对一个技术长期的研究，对自身不断优化的过程中，自我否定，在进行一次革新

  http://mike-neck.github.io/blog/2013/06/21/how-to-publish-artifacts-with-gradle-maven-publish-plugin-version-1-dot-6/

## 一. Android
### 1.1 通用Android技能说明

* [Fastboot 刷机教程](http://comsince.github.io/unbuntu-androrid-footboot)
* [Unbuntu android 环境初始配置](http://comsince.github.io/unbuntu-android-environment)
* [Android源码在线预览](http://www.grepcode.com/)
* [Android源码搜索](http://androidxref.com)

### 1.2 网络相关
#### 1.2.1 Push推送研究
  这里列举当前推送平台的发展趋势以及现有的平台提供的功能，之后会结合上述分析根据实际的经验分析目前我们所采用的push的实现方式

* [第三方推送平台及SDK汇总分析](http://comsince.github.io/Push-baidu-getui-compair)
* [第三方PushSDK 设计实践](http://comsince.github.io/push-design-thirdparty-doc)

#### 1.2.2 数据统计
   这里主要说明数据统计埋点信息的设计原则，以及如果提高数据上报的准确率
   数据统计设计模式说明
  

## 二. Web
### 2.1概述
这里主要说明web开发的发展动向以及一些主流的开发框架

* [分布式Web开发概述](http://comsince.github.io/spring_core_framework)


## 三. 开源项目源码分析与扩展
具体Android项目的分析说明详见[Android源码解析内容](http://comsince.github.io/Android-tech-doc)

## 四. 设计模式
 关于设计模式从各种开源项目中都能看到，它代表一种解决问题的方案，在众多的开源项目中，如果一开始就知道其实现的思想，对于理解其架构有很大的帮助

* [设计模式说明](./designpattern)

## 五. 插件加载APK，实现自动升级

