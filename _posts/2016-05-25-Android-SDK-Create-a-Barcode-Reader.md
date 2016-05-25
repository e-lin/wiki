---
layout: post
title:  "Android SDK: Create a Barcode Reader"
date:   2016-05-25 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
We use [Zxing Library][R2] to implement this.
<!--more-->

Steps
---

### 1. Create a New Android Project.

Let's call the Application Name: BardcodeScanningApp. Choose a blank activity.

Add one Button, two TextView in your main layout file, under the default <RelativeLayout>

``` xml
<Button
    android:id="@+id/scan_button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_centerHorizontal="true"
    android:text="@string/scan"
    android:onClick="sendBarcode"/>

<TextView
    android:id="@+id/scan_format"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textIsSelectable="true"
    android:layout_centerHorizontal="true"
    android:layout_below="@id/scan_button" />

<TextView
    android:id="@+id/scan_content"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textIsSelectable="true"
    android:layout_centerHorizontal="true"
    android:layout_below="@id/scan_format"/>
```

Looks like below, `android:onClick="sendBarcode"` is for adding `sendBarcode()` in the main activity code.

![CodeImg](http://i.imgur.com/m5JetjG.png)

### 2. Add ZXing to Your Project.

Add the following to your `build.gradle` file:

```
repositories {
    mavenCentral()

    maven {
        url "http://dl.bintray.com/journeyapps/maven"
    }
}

dependencies {
    // Supports Android 4.0.3 and later (API level 15)
    compile 'com.journeyapps:zxing-android-embedded:2.0.1@aar'

    // Supports Android 2.1 and later (API level 7), but not optimal for later Android versions.
    // If you only plan on supporting Android 4.0.3 and up, you don't need to include this.
    compile 'com.journeyapps:zxing-android-legacy:2.0.1@aar'

    // Convenience library to launch the scanning and encoding Activities.
    // It automatically picks the best scanning library from the above two, depending on the
    // Android version and what is available.
    compile 'com.journeyapps:zxing-android-integration:2.0.1@aar'

    // Version 3.0.x of zxing core contains some code that is not compatible on Android 2.2 and earlier.
    // This mostly affects encoding, but you should test if you plan to support these versions.
    // Older versions e.g. 2.2 may also work if you need support for older Android versions.
    compile 'com.google.zxing:core:3.0.1'
}
```

### 3. Do Some Scanning.

In app's main activity class, above `onCreate()` method, add variables to represent the button and two Textviews we created in the layout file.

``` java
private Button scanBtn;
private TextView formatTxt, contentTxt;
```

In `onCreate()`, after the existing code, instantiate these variables using the ID values we specified in the XML.

```
scanBtn = (Button)findViewById(R.id.scan_button);
formatTxt = (TextView)findViewById(R.id.scan_format);
contentTxt = (TextView)findViewById(R.id.scan_content);
```

Now, create a `sendBarcode()` method.

``` java
    public void sendBarcode(View view){
        if ( view == scanBtn ){
            IntentIntegrator scanIntegrator = new IntentIntegrator(this);

            //scanning
            scanIntegrator.initiateScan();
        }
    }
```
At this point, the scanner will start. When you call the initiateScan method, you can choose to pass a collection of the barcode types you want to scan. By default, the method will scan for all supported types. These include UPC-A, UPC-E, EAN-8, EAN-13, QR Code, RSS-14, RSS Expanded, Data Matrix, Aztec, PDF 417, Codabar, ITF, Codes 39, 93, and 128.

### 4. Retrieve Scanning Results.

When the user clicks the scan button, the barcode scanner will launch. When they scan a barcode, it will return the scanned data to the onActivityResult method of the calling activity. Add the method to your main activity class.

``` java
public void onActivityResult(int requestCode, int resultCode, Intent intent){
    //retrieve scan result
    IntentResult scanningResult = IntentIntegrator.parseActivityResult(requestCode, resultCode, intent);

    if ( null != scanningResult ){
        //we have a result
        String scanContent = scanningResult.getContents();
        String scanFormat = scanningResult.getFormatName();

        formatTxt.setText("FORMAT: " + scanFormat);
        contentTxt.setText("CONTENT: " + scanContent);
    } else {
        Toast toast = Toast.makeText(getApplicationContext(), "No scan data received!", Toast.LENGTH_SHORT);
        toast.show();
    }
}
```

Results
---
![Imgur1](http://i.imgur.com/JPVOm44.png)
![Imgur2](http://i.imgur.com/Wi1AVZJ.png)
![Imgur3](http://i.imgur.com/zjSoNSH.png)

Reference
---
[Android SDK: Create a Barcode Reader][R1]<br />
[如何在Android Studio中加入library??][R3]

[R1]: http://code.tutsplus.com/tutorials/android-sdk-create-a-barcode-reader--mobile-17162
[R2]: https://github.com/zxing/zxing
[R3]: http://lazycatnote.blogspot.jp/2015/03/androidqr-code.html