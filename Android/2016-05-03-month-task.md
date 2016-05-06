---
layout: post
title: 五月份任务规划
category: opinion
description: 描述五月份任务的计划以及技术点上报
---

## 概述
主要针对实际工作中能够作为公共解决的问题进行经验汇总

## 一. Notification 多样化展示问题
push消息对通知栏消息的多样化提出了更高的要求，需要根据用户的需求展示多样化的通知栏
### 1.1 开源项目介绍
google的demo向来是比较权威的
* [android-CustomNotifications](https://github.com/googlesamples/android-CustomNotifications)
Define custom layouts for collapsed and expanded views.
* [Android-Notification-Example](https://github.com/saulmm/Android-Notification-Example)
   Simple notification.
   Expandable notification
   Progress notification
   Action button notification
* [android-BasicNotifications](https://github.com/googlesamples/android-BasicNotifications)



## 二. Notification 展示逻辑
Notification展示需要一定的时机，这里做展示时机说明


## 三 PushTracker 实时上报统计需求
### 功能概述
数据上报功能，主要是要保证数据能够在较短时间内准确上报
### 3.功能说明
#### 3.1.1 单条数据实时上报功能
对于某个单条数据需要马上上报，提供即时上报的功能
#### 3.1.2 上传失败数据自动保存功能
上传失败数据自动保存功能，网络连通自动上报功能，WIFI情况下批量上传;充电联网自动上报功能
#### 3.1.3 触发即时上报数据的功能
即时上报数据是指用户在完成某个指定的动作后，需要强行触发上报数据，比如PushSDK完成通知栏弹出后，确认这一系列动作完成后需要即刻上报数据

### 3.2 模块设计说明
此功能的设计与具体的数据上传接口无关，只需要定义相关的数据格式，实现向应用的event即可实现数据的上传
#### 3.2.1 EventStore
消息存储模块，负责消息存储；成功消息自动清除，失败消息自动缓存
#### 3.2.2 Emitter
消息上传模块，接收DataLoad数据
#### 3.2.3 DataLoad
Event消息转义模块，方便消息存储与扩展
#### 3.2.4 Event
定义Event消息基本结构与构造方法，方便创建Event消息
#### 3.2.5 Tracker
数据统计初始化入口，提供各类数据统计接口