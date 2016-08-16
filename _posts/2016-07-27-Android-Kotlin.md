---
layout: post
title:  "Android Kotlin"
date:   2016-07-27 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Why Kotlin?
---

- 100% interoperable with Java - Kotlin and Java can co-exist in one project. You can continue to use existing libraries in Java.
- Concise - Drastically reduce the amount of boilerplate code you need to write.
- Safe - Avoid entire classes of errors such as null pointer exceptions.
- It's functional - Kotlin uses many concepts from functional programming, such as lambda expressions.


Quick Start
---
#1. Create a new Android project.

#2. Modify the Gradle scripts to include the Kotlin Gradle plugin and standard library.

```
buildscript {
    ext.kotlin_version = '1.0.0'
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.5.0'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}
```

```
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'

dependencies {
    compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
}
```

#3. Apply the Kotlin plugin for IntelliJ or Studio.

Installing the IDE plugin is just like any other. You can find that under Preferences --> Plugins --> Install JetBrains plugin. Be sure to restart the IDE after installation.

#4. Convert a Java class to Kotlin.

Go find the main activity that was generated for you, select it in the project explorer on the left, and invoke the IDE action called “Convert Java File to Kotlin File”. You can do that by bringing up the action selector with Command-Shift-A (on OSX), then typing that action name. The plugin also provides a finger-bending keyboard shortcut for this action (on OSX it’s Option-Shift-Command-K).


Reference
---
[Kotlin & Android: A Brass Tacks Experiment, Part 1][R1]<br />
[Using Kotlin for Android development][R2]


[R1]: https://medium.com/@CodingDoug/kotlin-android-a-brass-tacks-experiment-part-1-3e5028491bcc#.npq7w4j5d
[R2]: https://github.com/codepath/android_guides/wiki/Using-Kotlin-for-Android-development
