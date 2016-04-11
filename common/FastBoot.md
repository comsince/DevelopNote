---
layout: post
title: "FastBoot 刷机教程"
description: 刷机基本原理，一般步骤，fastboot模式
category: blog
---

## 一 基本原理
### 1.1 android的分区知识
* (1) splash1
开机画面，使用Nandroid backup备份系统后的文件为splash1.img
* (2) recovery
该分区是恢复模式(即开机按Home+power进入的界面)，使用Nandroid backup备份为recovery.img
* (3) boot
内核启动分区，使用Nandroid backup备份为boot.img
* (4) system
Android系统部分，目录表示为/system，通常为只读，使用Nandroid backup备份为system.img
* (5) cache
 缓存文件夹，目录表示为/cache，事实上除了T-mobile的OTA更新外，别无用处，使用Nandroid backup备份为cache.img
* (6) userdata
用户安装的软件以及各种数据，目录为/data，使用Nandroid backup备份为data.img 

## 二 刷机的一般步骤
* (1) 修改开机画面, 修改的是splash1
* (2) root时刷的是所有分区
* (3) 刷test_keys，更新的应该是recovery
* (4) 使用update.zip刷是更新boot、system
* (5) 恢复出厂设置, 清空的是userdata和cache

明白这些之后就很好理解,一般无须更新recovery.IMG,正常情况下只需要更新BOOT和SYSTEM即可

## 三 fastboot模式
### 3.1 用cd命令打开fastboot所在的文件夹
 备注
 fastboot是Android SDK自带工具，在Android SDK目录的platform-tools/子目录
 
### 3.2 擦除数据
```
    删除boot数据
    fastboot erase boot
    删除system数据
    fastboot erase system
    删除userdata数据
    fastboot erase userdata 
    删除recouvery数据
    fastboot erase recovery
```
### 3.3 写入img文件

```
    写入boot.img
    fastboot flash boot boot.img
    写入system.img
    fastboot flash system system.img 
```
> 注意boot.img及system.img的路径

### 3.4 recovery 与userdata重刷入

```
   fastboot flash userdata userdata.Img（确定在备份里面是这个名字或者之前又这个分区）
   fastboot flash recovery recovery.img
``` 

### 3.5 重启

```
   fastboot reboot 
```
-- [原文链接](http://bbs.imobile.com.cn/thread-tid-8559825.html)

