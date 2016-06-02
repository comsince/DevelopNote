---
layout: post
title: "Android Test"
description: Android unit test,UI test
category: blog
---

## 概述
### Android Test 现有技术支持
* [Getting startedf with testing](https://developer.android.com/training/testing/start/index.html)
  介绍Android 测试的两种测试，本地模拟测试和真机测试
* [A collection of samples demonstrating different frameworks and techniques for automated testing](https://github.com/googlesamples/android-testing)

* [Android Testing Support Library](https://google.github.io/android-testing-support-library/docs/index.html)

## Android Unti Test Type
### Local Unit Tests
  * Dependence build.gradle
```
	dependencies {
	    // Required -- JUnit 4 framework
	    testCompile 'junit:junit:4.12'
	    // Optional -- Mockito framework
	    testCompile 'org.mockito:mockito-core:1.10.19'
	}
```
### Instrumented Tests
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

  * run intrument test on command line
