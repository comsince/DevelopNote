---
layout: post
title: gradle tools for android develop
category: opinion
description: gradle for create android plugin and for unit test...
---

## create gradle plugin
### [Build script](https://github.com/adavis/caster-io-samples/tree/master/GradlePluginBasics)
You can include the source for the plugin directly in the build script. This has the benefit that the plugin is automatically compiled and included in the classpath of the build script without you having to do anything. However, the plugin is not visible outside the build script, and so you cannot reuse the plugin outside the build script it is defined in. 
### [buildSrc project](https://github.com/adavis/caster-io-samples/tree/master/GradlePluginIntermediate)
You can put the source for the plugin in the rootProjectDir/buildSrc/src/main/groovy directory. Gradle will take care of compiling and testing the plugin and making it available on the classpath of the build script. The plugin is visible to every build script used by the build. However, it is not visible outside the build, and so you cannot reuse the plugin outside the build it is defined in.
 See Chapter 41, Organizing Build Logic for more details about the buildSrc project.
### [Standalone project](https://github.com/adavis/caster-io-samples/tree/master/sample-plugin)
You can create a separate project for your plugin. This project produces and publishes a JAR which you can then use in multiple builds and share with others. Generally, this JAR might include some custom plugins, or bundle several related task classes into a single library. Or some combination of the two. 

### write custom android gradle plugin
* [Create a Standalone Gradle plugin for Android - a step-by-step guide ](https://afterecho.uk/blog/create-a-standalone-gradle-plugin-for-android-a-step-by-step-guide.html)
* [Create a Standalone Gradle plugin for Android - part 2 ](https://afterecho.uk/blog/create-a-standalone-gradle-plugin-for-android-part-2.html)
* [Create a Standalone Gradle plugin for Android - part 3 ](https://afterecho.uk/blog/create-a-standalone-gradle-plugin-for-android-part-3.html)

### Android Unit Test
* Condition of Android unit test Gradle plugin
Version 1.1 of Android Studio and the Android gradle plugin brings support for unit testing your code on your development computer
* [Android unit test support](http://tools.android.com/tech-docs/unit-testing-support#)
  * Good News
    Version 1.1 of Android Studio and the Android gradle plugin brings support for unit testing your code on your development computer.
  * Test code dependd on platform api have problem
    The android.jar file that is used to run unit tests does not contain any actual code - that is provided by the Android system image on real devices. Instead, all methods throw exceptions (by default). This is to make sure your unit tests only test your code and do not depend on any particular behaviour of the Android platform (that you have not explicitly mocked e.g. using Mockito). If that proves problematic, you can add the snippet below to your build.gradle to change this behavior

* Solution for testing code depend on Android platform api
  * [why Android unit test so difficult](https://segmentfault.com/a/1190000002904944)
  * [sample-android-testing](https://github.com/adavis/sample-android-testing)

  * [美团技术博客-Android单元测试研究与实践](http://tech.meituan.com/Android_unit_test.html)
  * [Android test option overview](https://github.com/codepath/android_guides/wiki/Android-Testing-Options)
  Automated Testing is an important topic that helps us ensure quality when building Android apps. There are many different testing tools and frameworks we can use while developing Android apps. This guide will take a look at some of the more popular approaches available. For someone first starting with testing, we recommend looking at Robolectric for unit testing, Espresso for UI testing, Assertj-Android for better validation support, and Mockito for mocking.
  * [Unit Testing with Robolectric](https://github.com/codepath/android_guides/wiki/Unit-Testing-with-Robolectric)


### write custom gradle plugin
* [write custom plugin with task](https://www.javacodegeeks.com/2012/08/gradle-custom-plugin.html)
* [Gradle Goodness: Pass Command-line Arguments to Build Script](http://mrhaki.blogspot.com/2010/10/gradle-goodness-pass-command-line.html)  