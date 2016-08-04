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
* [Basic auth Ȩ����֤](https://wiki.jenkins-ci.org/display/JENKINS/Authenticating+scripted+clients)

## 2.2 jenkins job and plugin
### 2.2.1 [Remote access API](https://wiki.jenkins-ci.org/display/JENKINS/Remote+access+API)
Remote API can be used to do things like these:
* retrieve information from Jenkins for programmatic consumption
* trigger a new build

   ```
    curl -X POST http://username:token@jenkins.rnd.meizu.com/view
    /StandAlone/job/StandAlone_InternetPlatform_Common/build?token=common
   ```

* create/copy jobs

### 2.2.2 Jenkins Aritfactory Plugin
* [Artifactory Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Artifactory+Plugin)
  jenkins ���õ���չ�ԣ��кܶ���صĲ����������ʹ�ã�artifactory plugin ���Լ���artifactory�Զ������Ĺ��ܣ�����������Ҫʹ��artifactory����Ĺ���Ĭ�϶�ȡ����Ա�˻�����Ϣ����gradle����Զ�����aar��

# �� Gradle 
## 3.1 Gradle Plugin
   ����gradle plugin���õ���չ�ԣ����л��������̹淶��Ŀǰ���ԵĻ����������£�
   �������ϴ�����->����jenkin�Զ�����������->�Զ����д������apk->����ATS����ƽ̨���е�Ԫ����->�ϴ����Խ��->�Զ�����aar

   
## 3.2 Type of Gradle Plugin
### 3.2.1 [Build script](https://github.com/adavis/caster-io-samples/tree/master/GradlePluginBasics)
You can include the source for the plugin directly in the build script. This has the benefit that the plugin is automatically compiled and included in the classpath of the build script without you having to do anything. However, the plugin is not visible outside the build script, and so you cannot reuse the plugin outside the build script it is defined in. 
### 3.2.2 [buildSrc project](https://github.com/adavis/caster-io-samples/tree/master/GradlePluginIntermediate)
You can put the source for the plugin in the rootProjectDir/buildSrc/src/main/groovy directory. Gradle will take care of compiling and testing the plugin and making it available on the classpath of the build script. The plugin is visible to every build script used by the build. However, it is not visible outside the build, and so you cannot reuse the plugin outside the build it is defined in.
 See Chapter 41, Organizing Build Logic for more details about the buildSrc project.
### 3.2.3 [Standalone project](https://github.com/adavis/caster-io-samples/tree/master/sample-plugin)
You can create a separate project for your plugin. This project produces and publishes a JAR which you can then use in multiple builds and share with others. Generally, this JAR might include some custom plugins, or bundle several related task classes into a single library. Or some combination of the two. 

## 2.3 write custom android gradle plugin
* [Create a Standalone Gradle plugin for Android - a step-by-step guide ](https://afterecho.uk/blog/create-a-standalone-gradle-plugin-for-android-a-step-by-step-guide.html)
* [Create a Standalone Gradle plugin for Android - part 2 ](https://afterecho.uk/blog/create-a-standalone-gradle-plugin-for-android-part-2.html)
* [Create a Standalone Gradle plugin for Android - part 3 ](https://afterecho.uk/blog/create-a-standalone-gradle-plugin-for-android-part-3.html)


## 3.2 ����gradleʵ�ֵ�Ԫ���Բ��
* [Platform_Gradle�����Ŀ](http://gitlab.meizu.com/liaojinlong/Platform_Gradle)

# �� Android Test Support Library
## 4.1 Android Test ���м���֧��
* [Getting startedf with testing](https://developer.android.com/training/testing/start/index.html)
  ����Android ���Ե����ֲ��ԣ�����ģ����Ժ��������
* [A collection of samples demonstrating different frameworks and techniques for automated testing](https://github.com/googlesamples/android-testing)
* [Android Testing Support Library](https://google.github.io/android-testing-support-library/docs/index.html)

## 4.2 Android ��Ԫ��������
### 4.2.1 ���ص�Ԫ����
  * Dependence build.gradle
```
	dependencies {
	    // Required -- JUnit 4 framework
	    testCompile 'junit:junit:4.12'
	    // Optional -- Mockito framework
	    testCompile 'org.mockito:mockito-core:1.10.19'
	}
```
### 4.2.2 ���ģ�����
  * Dependence build.gradle

```
    androidTestCompile 'com.android.support:support-annotations:23.0.1'
    androidTestCompile 'com.android.support.test:runner:0.4.1'
    androidTestCompile 'com.android.support.test:rules:0.4.1'
    // Optional -- Hamcrest library
    androidTestCompile 'org.hamcrest:hamcrest-library:1.3'
    // Optional -- UI testing with Espresso
    androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.1'
    // Optional -- UI testing with UI Automator
    androidTestCompile 'com.android.support.test.uiautomator:uiautomator-v18:2.1.1'

```
  * build.gradle defaultConfig
```
	android {
	    defaultConfig {
		testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
	    }
	}

```
