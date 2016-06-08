---
layout: post
title:  "Android Cheatsheet"
date:   2016-05-24 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Android Studio
---
- `opt + return` to import missing classes.
- `cmd + o` to navigate to Class.
- `sht + cmd + o` to navigate to File.
- `opt + cmd + o` to navigate to Symbol.
- `cmd + l` to navigate to Line.
- `cmd + fn + F9` to build program.
- `ctr + r` to run application.
- `ctr + opt + r` to run program.


Get Device Information
---

``` java
// get device information
String s = "Debug-infos";
s += "\nOS Version: " + System.getProperty("os.version") + "(" + Build.VERSION.INCREMENTAL + ")";
s += "\nOS API Level: " + Build.VERSION.SDK_INT;
s += "\nDevice: " + Build.DEVICE;
s += "\nBrand: " + Build.BRAND;
s += "\nManufacturer: " + Build.MANUFACTURER;
s += "\nModel (and Product): " + Build.MODEL + "(" + Build.PRODUCT + ")";
Log.v("Device Info:", s);
```

Result would be like this:

```
V/Device Info: Debug-infos
               OS Version: 3.10.0-genymotion-g08e528d(eng.buildbot.20160110.195928)
               OS API Level: 23
               Device: vbox86p
               Brand: Android
               Manufacturer: Genymotion
               Model (and Product): Google Nexus 6P - 6.0.0 - API 23 - 1440x2560(vbox86p)
```

#### Reference

- [How to detect system information like os or device type][R1]
- [Build - Android Developers][R2]


Get Current timestamp
---

``` java
Long tsLong = System.currentTimeMillis()/1000;
String ts = tsLong.toString();
```

I do not use `TimingLogger` as it is more suitable for code execution time. Besides, developers have to set a system property like:

```
$ adb shell
# setprop
usage: setprop <key> <value>
# setprop log.tag.MyTag VERBOSE
#
```
to make `Log.isLoggable` become true. Otherwise the `addSplit` and `dumpToLog` call will do nothing.


#### Reference

- [Android Get Current timestamp?][R3]
- [Time code execution in Android][R4]


Get the current language in device
---

``` java
Locale.getDefault().getDisplayLanguage();
```

Someone has checked the Locale methods on an Android 4.1.2 device, and the results:

``` java
Locale.getDefault().getLanguage()       ---> en
Locale.getDefault().getISO3Language()   ---> eng
Locale.getDefault().getCountry()        ---> US
Locale.getDefault().getISO3Country()    ---> USA
Locale.getDefault().getDisplayCountry() ---> United States
Locale.getDefault().getDisplayName()    ---> English (United States)
Locale.getDefault().toString()          ---> en_US
Locale.getDefault().getDisplayLanguage()---> English
```

#### Reference

- [Get the current language in device][R5]


Make a clickable URL in Textview and return true if the URL was clicked
---

Add a `TextView` in layout.xml

``` xml
    <TextView
        android:id="@+id/my_link"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:autoLink="web"
        android:onClick="clickURL"/>
```

`android:autoLink="web"` will find an URL and create a link even if android:linksClickable is not set, links are by default clickable. You don't have to keep the URL alone, even in the middle of a text it will be detected and clickable.

To set a link via the code, same principle, no need for pattern or android:autoLink in layout, the link is found automatically using Linkify:

``` java
TextView text_view = (TextView) findViewById(id.my_link);
text_view.setText("Click my web site: www.stackoverflow.com");
Linkify.addLinks(text_view, Linkify.WEB_URLS);
```

``` java
public void clickURL(View view) {

    //do something you like while the Textview is clicked
    ifClick = true;
}
```

#### Reference

- [Android active link of url in TextView][R6]
- [setOnClickListener on TextView][R7]
- [Android WebView - Intercept clicks][R8]
- [Android TextView with Clickable Links: how to capture clicks?][R9]




[R1]: http://stackoverflow.com/questions/3213205/how-to-detect-system-information-like-os-or-device-type
[R2]: https://developer.android.com/reference/android/os/Build.html
[R3]: http://stackoverflow.com/questions/8077530/android-get-current-timestamp
[R4]: http://stackoverflow.com/questions/3128181/time-code-execution-in-android
[R5]: http://stackoverflow.com/questions/4212320/get-the-current-language-in-device
[R6]: http://stackoverflow.com/questions/6910703/android-active-link-of-url-in-textview
[R7]: http://stackoverflow.com/questions/17630697/setonclicklistener-on-textview
[R8]: http://stackoverflow.com/questions/3250034/android-webview-intercept-clicks
[R9]: http://stackoverflow.com/questions/12418279/android-textview-with-clickable-links-how-to-capture-clicks




