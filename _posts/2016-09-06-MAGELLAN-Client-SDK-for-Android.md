---
layout: post
title:  "MAGELLAN Client SDK for Android"
date:   2016-09-06 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Before you use **MAGELLAN Client SDK for Android** in Android Studio, you must...

It is not enough to just read this link:
[Client SDK for Androidの導入手順][R1]

You still need to do the actions below:

Steps
---

### 1. Copy .so files

Create a folder named `jniLibs` under YourApp/app/src/main/, and copy the .so files under the jniLibs folder. The folder structure would be like this:

```
$ pwd
YourApp/app/src/main
```

```
$ tree
├── jniLibs
│   ├── armeabi-v7a
│   │   └── libmagellan_java.so
│   └── x86
│       └── libmagellan_java.so
```

### 2. Add network permission

In YourApp/app/src/main/AndroidManifest.xml

add two lines like:

``` xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="planet.co.jp.multilingualscanner">

    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

    <application>
        ...
    </application>
```

Dish of the day
---
![Imgur](http://i.imgur.com/4OJweos.jpg)
#homemade

Reference
---
[Client SDK for Androidの導入手順][R1]

[R1]: http://devcenter.magellanic-clouds.com/reference/client-sdk/ja/android/introduction.html
