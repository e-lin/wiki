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
Solution: getStringExtra() should wrap under onCreate(), not out above onCreate(). The upper place where you write is executed when the constructor of the activity is invoked, at this point, the Intent obeject does not exist yet. Android does a lot of things after the constructor was invoked and then invokes onCreate.

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

[W/com.google.zxing.client.android.camera.open.OpenCameraInterface: No cameras!][R4]
---
Solution: You should enable the AVD's camera. In Android Studio, go to AVD Manager, click on edit (pencil) button in Actions. After that, in next appearing window, click on Advance settings, and then scroll down and in Camera options, select webcam for front and back.


[Unexpected error initializing camera java.lang.RuntimeException: Fail to connect to camera service][R5]
---
Solution: Getting camera error in Zxing barcode application, for Android 6+, because of the "permission" issue. If you got the message "Sorry, the camera encountered a problem. You may need to restart the device.", go to Settings - Apps - find "your app name" - select Permissions and switch on "Camera".


[Getting Error “Gradle DSL method not found: 'compile()'” when Syncing Build.Gradle][R6]
---
Solution: Dependencies should be put into the individual module's build.gradle files rather than at the top most level build.gradle file. In my case, that means the dependency should be added to the `app` module's build.gradle file.

```
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    testCompile 'junit:junit:4.12'
    compile files('libs/magellan.jar')
    compile 'com.android.support:appcompat-v7:22.2.1'
}
```


[No resource found that matches the given name 'Theme.AppCompat.Light.DarkActionBar'.][R7]
---
Solution: AppCompat is a library project. You need to reference the library project in your android project.
Check the topic [Adding libraries with resources][R8].


[Error retrieving parent for item: No resource found that matches the given name after upgrading to AppCompat v23][R9]
---
Solution: Your compile SDK version must match the support library's major version. Since you are using version 23 of the support library, you need to compile against version 23 of the Android SDK. Alternatively you can continue compiling against version 22 of the Android SDK by switching to the latest support library v22.

change

```
dependencies {
    compile 'com.android.support:appcompat-v7:23.4.0'
}
```
back to

```
dependencies {
    compile 'com.android.support:appcompat-v7:22.2.1'
}
```


[java.lang.NullPointerException: Attempt to get length of null array][R10]
---

``` java
import net.magellan.BaseRequest;

public void onError(BaseRequest req){

    Log.v("Worker", new String(req.getResponseData())); // ---> here got a null pointer
}
```
Solution: The problem is because getResponseData() return null.


[I/Choreographer: Skipped 48 frames!  The application may be doing too much work on its main thread.][R11]
---
Solution: ongoing...


[java.lang.IllegalStateException: Cannot execute task: the task has already been executed (a task can be executed only once)][]
---
Solution: When using AsyncTask, you cannot call `execute();` twice. But if you new an AsyncTask everytime, you should consider how to claim your AsyncTask. Afterall, you have to stop all the AsyncTasks in the onStop() of activity.


[Fatal signal 11 (SIGSEGV), code 1, fault addr 0x0 in tid 4438 (Timer-20)][R13]
---
Solution: This is memory related error. You may have a memory leak or out of memory in your device. Check if your stop thread before it goes unclaimed.




[R1]: http://stackoverflow.com/questions/34814368/gradle-version-2-10-is-required-error
[R2]: http://stackoverflow.com/questions/5526470/trying-to-add-adb-to-path-variable-osx
[R3]: https://plus.google.com/+ShakaHuang/posts/dQiRid51aFD
[R4]: http://stackoverflow.com/questions/27875415/android-emulator-unable-to-start-webcam-to-capture-picture-in-emulator
[R5]: http://stackoverflow.com/questions/9028735/getting-camera-error-in-zxing-barcode-application
[R6]: http://stackoverflow.com/questions/27156428/getting-error-gradle-dsl-method-not-found-compile-when-syncing-build-grad
[R7]: http://stackoverflow.com/questions/21900853/no-resource-found-theme-appcompat-light-darkactionbar
[R8]: https://developer.android.com/topic/libraries/support-library/setup.html#add-library
[R9]: http://stackoverflow.com/questions/32075498/error-retrieving-parent-for-item-no-resource-found-that-matches-the-given-name
[R11]: http://stackoverflow.com/questions/14678593/the-application-may-be-doing-too-much-work-on-its-main-thread
[R13]: http://stackoverflow.com/questions/17840521/android-fatal-signal-11-sigsegv-at-0x636f7d89-code-1-how-can-it-be-tracked


