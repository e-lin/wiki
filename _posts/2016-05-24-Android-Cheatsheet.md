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
- `cmd + n` to add a new method.
- `cmd + b` to find declaration and go to.
- `cmd + e` to open recent edited files.
- `opt + fn + F7` to find usage.
- `cmd + opt + <-` to back to the last position where the cursor pointed.
- `cmd + d` to copy and paste a line.
- `cmd + sft + f` to find in path.

Gradle: Add shared project
---
In app's `build.gradle`, add:

```
dependencies {
    compile project(':zxing-android-embedded')
}
```

#### Reference

- [24.7. Project lib dependencies][R15]


Get device information
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


Get UTC timestamp
---

``` java
Long tsLong = System.currentTimeMillis()/1000;
String ts = tsLong.toString();
```

I do not use `TimingLogger` as it is more suitable for calculating code execution time. We are not measuring code execution time here. Besides, developers have to set a system property like:

```
$ adb shell
# setprop
usage: setprop <key> <value>
# setprop log.tag.MyTag VERBOSE
#
```
to make `Log.isLoggable` become true. Otherwise the `addSplit` and `dumpToLog` call will do nothing.

Use getTime() of Date object,

``` java
Date date = new Date();
long timestamp = date.getTime();
```
you will also get time in millisecond like `System.currentTimeMillis()` in 12 digits.
To get 10 digits of timestamp, try `date.getTime()/1000`.

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


Get device screen dpi
---

``` java
DisplayMetrics metrics = getResources().getDisplayMetrics();
int densityDpi = (int)(metrics.density * 160f);
```
`metrics.densityDpi` property will be one of the constants (120, 160, 213, 240, 320, 480 or 640 dpi).


#### Reference

- [getting the screen density programmatically in android?][R10]


Change locale at runtime
---

``` java
private Locale myLocale;
public void set_locale(String lang) {
      myLocale = new Locale(lang);
      Resources res = getResources();
      Configuration conf = res.getConfiguration();
      conf.locale = myLocale;
      res.updateConfiguration(conf, res.getDisplayMetrics());

      recreate();
//      Intent intent = new Intent(this, DisplayMessageActivity.class);
//      startActivity(intent);
```

For Traiditional Chinese or Simplified Chinese, you should rather write:

``` java
new Locale("zh", "TW")
```

#### Reference

- [Android localization at runtime][R11]
- [Change the locale at runtime?][R12]
- [Chinese not working][R13]


Content behind CoordinatorLayout AppBarLayout
---

If you created a NavigationDrawer with Android Studio's default template, you will find once you add contents in `content_xxx.xml` file, the contect end up being behind the AppBar(ToolBar).

To solve this, add `layout_behavior` to your ViewGroup(RelativeLayout, ScrollView, FrameLayout, etc.) like:

``` xml
xmlns:app="http://schemas.android.com/apk/res-auto"
app:layout_behavior="@string/appbar_scrolling_view_behavior"
```

#### Reference

- [Content behind CoordinatorLayout AppBarLayout][R14]


Alert Dialog
---
An example of making a dialog:

``` java
public void showAlertDialog() {
    AlertDialog.Builder builder = new AlertDialog.Builder(this);
    builder.setMessage(R.string.dialog_message).setTitle(R.string.dialog_tile);
    builder.setPositiveButton("OK", null);
    AlertDialog dialog = builder.create();
    dialog.show();
}
```
Result:

![Imgur](http://i.imgur.com/qrLFrPT.png)

#### Reference

- [Android - Alert Dialog Tutorial][R16]
- [Dialogs - Android Developers][R17]

Dialog Fragment
---
Dialog fragment is a fragment which can show fragment in dialog box.

To implement a dialog fragment:

#1. Create a new class like:

``` java
public static class MyAlertDialogFragment extends DialogFragment {

    public static MyAlertDialogFragment newInstance(int title) {
        MyAlertDialogFragment frag = new MyAlertDialogFragment();
        Bundle args = new Bundle();
        args.putInt("title", title);
        frag.setArguments(args);
        return frag;
    }

    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {
        int title = getArguments().getInt("title");

        return new AlertDialog.Builder(getActivity())
                .setIcon(R.drawable.alert_dialog_icon)
                .setTitle(title)
                .setPositiveButton(R.string.alert_dialog_ok,
                    new DialogInterface.OnClickListener() {
                        public void onClick(DialogInterface dialog, int whichButton) {
                            ((FragmentAlertDialog)getActivity()).doPositiveClick();
                        }
                    }
                )
                .setNegativeButton(R.string.alert_dialog_cancel,
                    new DialogInterface.OnClickListener() {
                        public void onClick(DialogInterface dialog, int whichButton) {
                            ((FragmentAlertDialog)getActivity()).doNegativeClick();
                        }
                    }
                )
                .create();
    }
}
```

#2. The activity creating this fragment may have the following methods to show the dialog and receive results from it:

``` java
void showDialog() {
    DialogFragment newFragment = MyAlertDialogFragment.newInstance(
            R.string.alert_dialog_two_buttons_title);
    newFragment.show(getFragmentManager(), "dialog");
}

public void doPositiveClick() {
    // Do stuff here.
    Log.i("FragmentAlertDialog", "Positive click!");
}

public void doNegativeClick() {
    // Do stuff here.
    Log.i("FragmentAlertDialog", "Negative click!");
}
```

More flexible way to implement:

- [DialogFragment 実装パターン][R19]
- [Using DialogFragment][R20]

#### Reference

- [DialogFragment][R18]



[R1]: http://stackoverflow.com/questions/3213205/how-to-detect-system-information-like-os-or-device-type
[R2]: https://developer.android.com/reference/android/os/Build.html
[R3]: http://stackoverflow.com/questions/8077530/android-get-current-timestamp
[R4]: http://stackoverflow.com/questions/3128181/time-code-execution-in-android
[R5]: http://stackoverflow.com/questions/4212320/get-the-current-language-in-device
[R6]: http://stackoverflow.com/questions/6910703/android-active-link-of-url-in-textview
[R7]: http://stackoverflow.com/questions/17630697/setonclicklistener-on-textview
[R8]: http://stackoverflow.com/questions/3250034/android-webview-intercept-clicks
[R9]: http://stackoverflow.com/questions/12418279/android-textview-with-clickable-links-how-to-capture-clicks
[R10]: http://stackoverflow.com/questions/3166501/getting-the-screen-density-programmatically-in-android
[R11]: http://programmerguru.com/android-tutorial/android-localization-at-runtime/
[R12]: http://stackoverflow.com/questions/13181847/change-the-locale-at-runtime
[R13]: https://github.com/ocpsoft/prettytime/issues/78
[R14]: http://stackoverflow.com/questions/32855889/content-behind-coordinatorlayout-appbarlayout
[R15]: https://docs.gradle.org/current/userguide/multi_project_builds.html#sec:multiproject_build_and_test
[R16]: http://www.tutorialspoint.com/android/android_alert_dialoges.htm
[R17]: https://developer.android.com/guide/topics/ui/dialogs.html
[R18]: https://developer.android.com/reference/android/app/DialogFragment.html
[R19]: http://qiita.com/kojionilk/items/9584c012679f61569995
[R20]: https://github.com/codepath/android_guides/wiki/Using-DialogFragment


