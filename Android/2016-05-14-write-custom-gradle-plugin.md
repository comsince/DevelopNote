---
layout: post
title: gradle tools for android develop
category: opinion
description: gradle for create android plugin and for unit test...
---

## create gradle plugin
### [Build script](https://github.com/adavis/caster-io-samples/tree/master/GradlePluginBasics)
    You can include the source for the plugin directly in the build script. This has the benefit that the plugin is automatically compiled and included in the
    classpath of the build script without you having to do anything. However, the plugin is not visible outside the build script, and so you cannot reuse the plugin
    outside the build script it is defined in. 
### [buildSrc project](https://github.com/adavis/caster-io-samples/tree/master/GradlePluginIntermediate)
    You can put the source for the plugin in the rootProjectDir/buildSrc/src/main/groovy directory. Gradle will take care of compiling and testing the plugin 
    and making it available on the classpath of the build script. The plugin is visible to every build script used by the build. However, it is not visible outside
    the build, and so you cannot reuse the plugin outside the build it is defined in.
    See Chapter 41, Organizing Build Logic for more details about the buildSrc project.
### [Standalone project](https://github.com/adavis/caster-io-samples/tree/master/sample-plugin)
    You can create a separate project for your plugin. This project produces and publishes a JAR which you can then use in multiple builds and share with others.
    Generally, this JAR might include some custom plugins, or bundle several related task classes into a single library. Or some combination of the two. 

### write custom android gradle plugin
* [Create a Standalone Gradle plugin for Android - a step-by-step guide ](https://afterecho.uk/blog/create-a-standalone-gradle-plugin-for-android-a-step-by-step-guide.html)
* [Create a Standalone Gradle plugin for Android - part 2 ](https://afterecho.uk/blog/create-a-standalone-gradle-plugin-for-android-part-2.html)
* [Create a Standalone Gradle plugin for Android - part 3 ](https://afterecho.uk/blog/create-a-standalone-gradle-plugin-for-android-part-3.html)

# android unit test
* [sample-android-testing](https://github.com/adavis/sample-android-testing)
  this project