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