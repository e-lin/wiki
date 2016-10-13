---
layout: post
title:  "Config the build using Build Variants and Proguard"
date:   2016-08-18 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Configure Build Variants
---
In `build.gradle` file you may define separate buildTypes like this:

```
buildTypes {
    release {
      runProguard false
      proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
      debuggable false
      versionNameSuffix "r"
    }

    debug {
      versionNameSuffix "d"
      debuggable true
    }

    test {
      applicationIdSuffix ".test"
      versionNameSuffix "t"
      debuggable false
    }
```
The above snippet achieves the following:

- config **release**, **debug**, and **test** builds
- set `versionNameSuffix` to refer to the version string which is build
- set `applicationIdSuffix` for may install **test** and **release** build on one device
- and most usefull **Gradle** Build Types can contribute to the build with resources: src/[buildtypename]/res. So you can have separate resource for separate builds.

For more info go to [http://tools.android.com/tech-docs/new-build-system/user-guide][R13]


Seperateing Configs in Java File to **gradle.properties**
---

For example, I have a `MyApplication.java`

``` java
public class MyApplication extends Application {

    private static final String PARSE_API_ID = "xxxxxxxxxxxxxxxxxxxxx";
    private static final String PARSE_API_KEY = "xxxxxxxxxxxxxxxxxxxxx";

    public MyApplication() {
        super();
    }

    @Override
    public void onCreate() {
        super.onCreate();
        Parse.initialize(this, PARSE_API_ID, PARSE_API_KEY);
        ParseInstallation.getCurrentInstallation().saveInBackground();
    }

}
```

#### Steps

#1. In `gradle.properties`, add:

```
parseApiId=xxxxxxxxxxxxxxxxxxxxx
parseApiKey=xxxxxxxxxxxxxxxxxxxxx
```

#2. In `build.gradle`, add:

```
buildTypes {
    debug {
        buildConfigField "String", "PARSE_API_ID", "\"${project.property("parseApiId")}\""
        buildConfigField "String", "PARSE_API_KEY", "\"${project.property("parseApiKey")}\""
    }
    release {
        buildConfigField "String", "PARSE_API_ID", "\"${project.property("parseApiId")}\""
        buildConfigField "String", "PARSE_API_KEY", "\"${project.property("parseApiKey")}\""
        minifyEnabled false
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
    }
}
```

#3. After syncing `build.gradle` file, **BuildConfig.java** will be generated automatically. `BuildConfig.java` would be like this:

``` java
public final class BuildConfig {
  // Fields from build type: debug
  public static final String PARSE_API_ID = "xxxxxxxxxxxxxxxxxxxxx";
  public static final String PARSE_API_KEY = "xxxxxxxxxxxxxxxxxxxxx";
}
```

#4. Apply to your `MyApplication.java` like:

``` java
public class MyApplication extends Application {

    public MyApplication() {
        super();
    }

    @Override
    public void onCreate() {
        super.onCreate();
        Parse.initialize(this, BuildConfig.PARSE_API_ID, BuildConfig.PARSE_API_KEY);
        ParseInstallation.getCurrentInstallation().saveInBackground();
    }

}
```

Configure the Build Process to Automatically Sign Your APK
---

#1. In your `gradle.properties` file, add:

```
# Signing configs
RELEASE_STORE_FILE = ../your_keystore.jks
RELEASE_STORE_PASSWORD = xxxx
RELEASE_KEY_ALIAS = xxxx
RELEASE_KEY_PASSWORD = xxxx
```

#2. Make sure `your_keystore.jks` file is under the root path of the project.

#3. In the `build.gradle` file, add:

```
signingConfigs {
    release {
        storeFile file(project.property("RELEASE_STORE_FILE"))
        storePassword project.property("RELEASE_STORE_PASSWORD")
        keyAlias project.property("RELEASE_KEY_ALIAS")
        keyPassword project.property("RELEASE_KEY_PASSWORD")
    }
```

and

```
buildTypes {
    release {
        signingConfig signingConfigs.release
    }
```
Make sure the code block of signingConfigs is above the code block of buildTypes, otherwise it cannot find `signingConfigs.release` when syncing the gradle.

#4. Open the **Build Variants** tool window and ensure that the release build type is selected.

#5. Click **Build > Build APK** to build your release build, and confirm that Android Studio has created a signed APK in the `build/outputs/apk/` directory for your module.

There you go! You will see

- app-release-unaligned.apk
- app-release.apk

under the path of `build/outputs/apk/`.


Shrink Your Code and Resources
---

To enable code shrinking with ProGuard, add `minifyEnabled true` to the appropriate build type in your `build.gradle` file. The `proguardFiles` property defines the ProGuard rules:

```
android {
    buildTypes {
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile(‘proguard-android.txt'),
                    'proguard-rules.pro'
        }
    }
    ...
}
```

Resource shrinking works only in conjunction with code shrinking. After the code shrinker removes all unused code, the resource shrinker can identify which resources the app still uses.

To enable resource shrinking, set the `shrinkResources` property to `true` in your `build.gradle` file (alongside `minifyEnabled` for code shrinking):

```
android {
    ...
    buildTypes {
        release {
            shrinkResources true
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'),
                    'proguard-rules.pro'
        }
    }
}
```

Troubles I Met
---

#### 1. After enabling `Proguard`, the proguard warning said that **can't find referenced class** and **can't find referenced method**.

The class that was pointed out is part of the Java runtime (rt.jar) but not part of the Android runtime (android.jar), so ProGuard warns that something might be broken. If you're sure that your application works anyway, you can specify

```
-dontwarn org.codehaus.mojo.animal_sniffer.IgnoreJRERequirement
-dontwarn java.nio.file.**
-dontwarn java.lang.invoke.**
-dontwarn java.lang.reflect.**
```
in your Proguard config file. Here I added this in `proguard-rules.pro`.


##### Glance at Stackoverflow

- [proguard hell - can't find referenced class][R6]
- [how to fix proguard warning 'can't find referenced method' for existing methods 'clone' and 'finalize' of class java.lang.Object][R7]
- [how to fix proguard warning 'can't find referenced method' for existing methods 'clone' and 'finalize' of class java.lang.Object][R8]

#### 2. Installing a signed release build with Proguard enabled into the device, after some operations getting crash error: **Proguard Missing type parameter** at **at java.lang.reflect.Method.invoke**.

`google/gson` has their example of proguard configuration for android applications.

[see on github][R11]

They propose to use this template of configuration:

```
##---------------Begin: proguard configuration for Gson  ----------
# Gson uses generic type information stored in a class file when working with fields. Proguard
# removes such information by default, so configure it to keep all of it.
-keepattributes Signature

# For using GSON @Expose annotation
-keepattributes *Annotation*

# Gson specific classes
-keep class sun.misc.Unsafe { *; }
#-keep class com.google.gson.stream.** { *; }

# Application classes that will be serialized/deserialized over Gson
-keep class com.google.gson.examples.android.model.** { *; }

##---------------End: proguard configuration for Gson  ----------
```

For our case we need to add this specifically, otherwise we get **at net.magellan.f.run(Unknown Source) at java.util.Timer$TimerImpl.run** with **Missing type parameter**.

```
-keep class net.magellan.** { *; }
```

##### Glance at Stackoverflow

- [proguard Missing type parameter][R9]
- [Proguard and reflection in Android][R10]

Speeding up Gradle build time
---

#1. Create a file named `gradle.properties` in the following directory:

- /home/<username>/.gradle/ (Linux)
- /Users/<username>/.gradle/ (Mac)
- C:\Users\<username>\.gradle (Windows)

#2. Add this line to the file:

```
org.gradle.daemon=true
```

DO NOT OVERLY MATCH YOUR PROGUARD RULE
---
@jakewharton gave a [talk][R14] on Aug 24, 2016, saying that

> Don’t use overly matchy rules. If you ever see * * in a ProGuard rules file, that is wrong 100% of the time. That is never what you should be using because you’ve effectively just nuked any advantages that you were getting from ProGuard.

Our proguard rule seems a bit inappropriate due to its overly match rules. But once I cross out the rule below:

```
#-keep class net.magellan.** { *; }
```
The app will crash with this message:

```
E/AndroidRuntime: FATAL EXCEPTION: Timer-5
                   Process: planet.co.jp.multilingualscanner, PID: 30833
                   java.lang.NoSuchMethodError: no static method "Lnet/magellan/Manager;._workerResponceCallback(JII[B[B)V"
                       at net.magellan.Manager.pollWorkerNativeEvent(Native Method)
                       at net.magellan.c.run(SourceFile:259)
                       at java.util.Timer$TimerImpl.run(Timer.java:284)
```

and

```
E/AndroidRuntime: FATAL EXCEPTION: Timer-0
                     Process: planet.co.jp.multilingualscanner, PID: 7274
                     java.lang.NoSuchMethodError: no static method "Lnet/magellan/Manager;._workerErrorCallback(JII[B[B)V"
                         at net.magellan.Manager.pollWorkerNativeEvent(Native Method)
                         at net.magellan.c.run(SourceFile:259)
                         at java.util.Timer$TimerImpl.run(Timer.java:284)
```
Add this to be more specific:

```
-keep class net.magellan.Manager { public static void _workerResponceCallback(long, int , int , byte[] , byte[] ); }
-keep class net.magellan.Manager { public static void _workerErrorCallback(long, int, int, byte[], byte[]); }
```


Today's Breakfest
---
![Imgur](http://i.imgur.com/35ZYkjo.jpg)


Reference
---
[Configure Build Variants][R1]<br />
[Shrink Your Code and Resources][R2]<br />
[gradle.propertiesに定義した値をJavaソースから参照する][R3]<br />
[Handling signing configs with Gradle][R4]<br />
[Sign Your App][R12]<br />
[How To… Speed up Gradle build time][R5]

[R1]: https://developer.android.com/studio/build/build-variants.html
[R2]: https://developer.android.com/studio/build/shrink-code.html
[R3]: http://qiita.com/shts/items/d94834437b22712415c5
[R4]: https://www.timroes.de/2013/09/22/handling-signing-configs-with-gradle/
[R5]: https://www.timroes.de/2013/09/12/speed-up-gradle/
[R6]: http://stackoverflow.com/questions/6974231/proguard-hell-cant-find-referenced-class
[R7]: http://stackoverflow.com/questions/23883028/how-to-fix-proguard-warning-cant-find-referenced-method-for-existing-methods
[R8]: http://stackoverflow.com/questions/23883028/how-to-fix-proguard-warning-cant-find-referenced-method-for-existing-methods
[R9]: http://stackoverflow.com/questions/8129040/proguard-missing-type-parameter
[R10]: http://stackoverflow.com/questions/4447145/proguard-and-reflection-in-android
[R11]: https://github.com/google/gson/tree/master/examples/android-proguard-example
[R12]: https://developer.android.com/studio/publish/app-signing.html#studio
[R13]: http://tools.android.com/tech-docs/new-build-system/user-guide
[R14]: https://realm.io/news/360andev-jake-wharton-java-hidden-costs-android/?utm_source=reddit.com&utm_medium=cpc&utm_campaign=java-hidden-costs

