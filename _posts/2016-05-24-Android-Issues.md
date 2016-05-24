---
layout: post
title:  "Android Issues"
date:   2016-05-24 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
If you encounter...
<!--more-->

[“Gradle Version 2.10 is required.” Error][R1]
---
Solution:
Set *Use default gradle wrapper* and edit `Project\gradle\wrapper\gradle-wrapper.properties` files field `distributionUrl` like this:

```
distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip
```

[Trying to add adb to PATH variable OSX][R2]
---
Solution:
Edit the `~/.bash_profile` like this:

```
export PATH="/Users/elsalin/Library/Android/sdk/platform-tools:$PATH"
export PATH="/Users/elsalin/Library/Android/sdk/tools:$PATH"
```

[emulator: ERROR: Unfortunately, there's an incompatibility between HAXM hypervisor and VirtualBox 4.3.30+ which doesn't allow multiple hypervisors to co-exist. It is being actively worked on; you can find out more about the issue at http://b.android.com/197915 (Android) and https://www.virtualbox.org/ticket/14294 (VirtualBox)][R3]
---
Solution:
In my case I was running a docker vm at same time, when stopped it android emulator launched successful.﻿

java.lang.NullPointerException: Attempt to invoke virtual method 'java.lang.String android.content.Intent.getStringExtra(java.lang.String)' on a null object reference
---
Solution: getStringExtra() should wrap under onCreate(), not out above onCreate().

``` java
public class DisplayMessageActivity extends AppCompatActivity {

// DO NOT PUT HERE
//    Intent intent = getIntent();
//    String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE);

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_display_message);

        Intent intent = getIntent();
        String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE);
```

[R1]: http://stackoverflow.com/questions/34814368/gradle-version-2-10-is-required-error
[R2]: http://stackoverflow.com/questions/5526470/trying-to-add-adb-to-path-variable-osx
[R3]: https://plus.google.com/+ShakaHuang/posts/dQiRid51aFD