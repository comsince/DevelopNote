---
layout: post
title: ���·�����滮
category: opinion
description: �������·�����ļƻ��Լ��������ϱ�
---

## ����
��Ҫ���ʵ�ʹ������ܹ���Ϊ���������������о������

## һ. Notification ������չʾ����
push��Ϣ��֪ͨ����Ϣ�Ķ���������˸��ߵ�Ҫ����Ҫ�����û�������չʾ��������֪ͨ��
### 1.1 ��Դ��Ŀ����
google��demo�����ǱȽ�Ȩ����
* [android-CustomNotifications](https://github.com/googlesamples/android-CustomNotifications)
Define custom layouts for collapsed and expanded views.
* [Android-Notification-Example](https://github.com/saulmm/Android-Notification-Example)
   Simple notification.
   Expandable notification
   Progress notification
   Action button notification
* [android-BasicNotifications](https://github.com/googlesamples/android-BasicNotifications)



## ��. Notification չʾ�߼�
Notificationչʾ��Ҫһ����ʱ����������չʾʱ��˵��

### ͼƬ�����뻺��
SDKΪ�˼��ٵ�����������ã����Զ���������������Ҫ�����Զ���ͼƬ����Ĳ���
* ��������
* ͼƬ����

## �� PushTracker ʵʱ�ϱ�ͳ������
### ���ܸ���
�����ϱ����ܣ���Ҫ��Ҫ��֤�����ܹ��ڽ϶�ʱ����׼ȷ�ϱ�
### 3.����˵��
#### 3.1.1 ��������ʵʱ�ϱ�����
����ĳ������������Ҫ�����ϱ����ṩ��ʱ�ϱ��Ĺ���
#### 3.1.2 �ϴ�ʧ�������Զ����湦��
�ϴ�ʧ�������Զ����湦�ܣ�������ͨ�Զ��ϱ����ܣ�WIFI����������ϴ�;��������Զ��ϱ�����
#### 3.1.3 ������ʱ�ϱ����ݵĹ���
��ʱ�ϱ�������ָ�û������ĳ��ָ���Ķ�������Ҫǿ�д����ϱ����ݣ�����PushSDK���֪ͨ��������ȷ����һϵ�ж�����ɺ���Ҫ�����ϱ�����

### 3.2 ģ�����˵��
�˹��ܵ���������������ϴ��ӿ��޹أ�ֻ��Ҫ������ص����ݸ�ʽ��ʵ����Ӧ�õ�event����ʵ�����ݵ��ϴ�
#### 3.2.1 EventStore
��Ϣ�洢ģ�飬������Ϣ�洢���ɹ���Ϣ�Զ������ʧ����Ϣ�Զ�����
#### 3.2.2 Emitter
��Ϣ�ϴ�ģ�飬����DataLoad����
#### 3.2.3 DataLoad
Event��Ϣת��ģ�飬������Ϣ�洢����չ
#### 3.2.4 Event
����Event��Ϣ�����ṹ�빹�췽�������㴴��Event��Ϣ
#### 3.2.5 Tracker
����ͳ�Ƴ�ʼ����ڣ��ṩ��������ͳ�ƽӿ�


## Gradle build tool
* gradle �������groovy�Ľű����������ű����ԣ��������ѡ��groovy��Ϊ������������(DSL)

###[Build Script Basics](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html)
Everything in Gradle sits on top of two basic concepts: projects and tasks.


### [Writing Build Scripts](https://docs.gradle.org/current/userguide/writing_build_scripts.html)
As the build script executes, it configures this Project object:
Getting help writing build scripts
Don't forget that your build script is simply Groovy code that drives the Gradle API. And the Project interface is your starting point for accessing everything in the Gradle API. So, if you're wondering what 'tags' are available in your build script, you can start with the documentation for the Project interface.
* Any method you call in your build script which is not defined in the build script, is delegated to the Project object.
* Any property you access in your build script, which is not defined in the build script, is delegated to the Project object.

### Dependence management
��������

### Gradle Plugins
Applying a plugin to a project allows the plugin to extend the project's capabilities. It can do things such as:
* Extend the Gradle model (e.g. add new DSL elements that can be configured)
* Configure the project according to conventions (e.g. add new tasks or configure sensible defaults)
* Apply specific configuration (e.g. add organizational repositories or enforce standards)

By applying plugins, rather than adding logic to the project build script, we can reap a number of benefits. Applying plugins:
* Promotes reuse and reduces the overhead of maintaining similar logic across multiple projects
* Allows a higher degree of modularization, enhancing comprehensibility and organization
* Encapsulates imperative logic and allows build scripts to be as declarative as possible

Plugins two types
* script plugin
* binary plugin