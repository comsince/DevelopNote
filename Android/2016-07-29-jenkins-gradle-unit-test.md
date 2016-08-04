---
layout: post
title: "Jenkins+Gradle Plugin+Android Unit Test"
description: jenkin api and gradle plugin for Android unit test
category: blog
---

# һ. ����
Android �Զ�����Ԫ����һֱ�����Ź�󿪷��ߣ�����˵��һ�ִ����ŵ�"����"�ļ�����������Ƭ���½������漸������˵�����������Զ����������ߴٽ�Android�Զ������Եľ���

* Jenkins�Զ�������
* Gradle Plugin�Զ�����������
* Android Unit Test ���Ը�����˵��

# ��. Jenkins
## 2.1 Jenkins �����
* [Jenkins Install](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins)
* [Securing Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Securing+Jenkins) 
  Jenkins ��ȫ����
  * [Basic auth Ȩ����֤](https://wiki.jenkins-ci.org/display/JENKINS/Authenticating+scripted+clients)
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
  jenkins ���õ���չ�ԣ��кܶ���صĲ����������ʹ�ã�artifactory plugin ���Լ���artifactory�Զ������Ĺ��ܣ�����������Ҫʹ��artifactory����Ĺ���Ĭ�϶�ȡ����Ա�˻�����Ϣ����gradle����Զ�����aar��
# �� Gradle 
## gradle plugin
   ����gradle plugin���õ���չ�ԣ����л��������̹淶��Ŀǰ���ԵĻ����������£�
   �������ϴ�����->����jenkin�Զ�����������->�Զ����д������apk->����ATS����ƽ̨���е�Ԫ����->�ϴ����Խ��->�Զ�����aar
## 2.1 ����gradleʵ�ֵ�Ԫ���Բ��

# �� Android Test Support Library
## 4.1
