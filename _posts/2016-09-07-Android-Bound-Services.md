---
layout: post
title:  "Android Bound Services"
date:   2016-09-07 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Creating a Bound Service
---
When creating a service that provides binding, you must provide an `IBinder` that provides the programming interface that clients can use to interact with the service. There are three ways you can define the interface:

- Extending the Binder class (service is private and runs in the same process as the client)
- Using a Messenger (service works across different processes)
- Using AIDL (more low-level)

Commonly we use the way of extending the binder class.


Extending the Binder Class
---

#1. Right click in your application package to new a Service, and name it whatever you like.

![Imgur](http://i.imgur.com/NiBRiMg.png)

Let's say we will have a `MagellanService`.

#2. Create an inner class which extends `Binder`, and make a method `getService()` to return the instance of `MagellanService` so clients can call public methods.

``` java
    public class MagellanBinder extends Binder {
        MagellanService getService() {
            return MagellanService.this;
        }
    }
```

You'll have it like:

![Imgur](http://i.imgur.com/3HSPa3p.png)

#3. Finish the defualt existed `onBind()` method, which returns the instance of Binder.

``` java
public class MagellanService extends Service {

    private final IBinder mBinder = new MagellanBinder();

    @Override
    public IBinder onBind(Intent intent) {
        return mBinder;
    }
}
```

#4. Create some public methods so that clients can call them.

``` java
    /** method for clients */
    public String getHiFromService() {
      return "Hi";
    }
```

There you go! You will be having this like:

![Imgur](http://i.imgur.com/nEVM5YN.png)


Create your Client
---

An example of an activity that binds to MagellanService and calls `getHiFromService()` when a button is clicked:

#1. Defines callbacks for service binding, passed to `bindService()`

``` java
public class BindingActivity extends Activity {
    MagellanService mService;
    boolean mBound = false;

    private ServiceConnection mConnection = new ServiceConnection() {

            @Override
            public void onServiceConnected(ComponentName className,
                    IBinder service) {
                // We've bound to MagellanService, cast the IBinder and get MagellanService instance
                MagellanBinder binder = (MagellanBinder) service;
                mService = binder.getService();
                mBound = true;
            }

            @Override
            public void onServiceDisconnected(ComponentName arg0) {
                mBound = false;
            }
        };
    }
```

#2. Bind to your Service

``` java
    @Override
    protected void onStart() {
        super.onStart();
        // Bind to your service
        Intent intent = new Intent(this, MagellanService.class);
        bindService(intent, mConnection, Context.BIND_AUTO_CREATE);
    }
```


- The first parameter of `bindService()` is an `Intent` that explicitly names the service to bind (thought the intent could be implicit).
- The second parameter is the `ServiceConnection` object.
- The third parameter is a flag indicating options for the binding. It should usually be `BIND_AUTO_CREATE` in order to create the service if its not already alive. Other possible values are `BIND_DEBUG_UNBIND` and `BIND_NOT_FOREGROUND`, or `0` for none.


#3. Unbind from the Service

``` java
    @Override
    protected void onStop() {
        super.onStop();
        // Unbind from the service
        if (mBound) {
            unbindService(mConnection);
            mBound = false;
        }
    }
```

#4. Attach the button in the layout file to this `onButtonClick()` method with the android:onClick attribute

``` java
    public void onButtonClick(View v) {
        if (mBound) {
            // Call a method from the MagellanService.
            // However, if this call were something that might hang, then this request should
            // occur in a separate thread to avoid slowing down the activity performance.
            String msg = mService.getHiFromService();
            Toast.makeText(this, "Hi from Service: " + msg, Toast.LENGTH_SHORT).show();
        }
    }
```

For more sample code, see the [LocalService.java][R5] class and the [LocalServiceActivities.java][R6] class in [ApiDemos][R4].


Important Notes about Binding to a Service
---

- Always trap **[DeadObjectException][R2]** exceptions, which are thrown when the connection has broken. This is the only exception thrown by remote methods.
- Objects are reference counted across processes.
- Usually pair the `binding` and `unbinding` for handling the client's lifecycle.
    - for `onStart()` + `onStop()`
    - for `onCreate()` + `onDestroy()` (heavy weight of the process, system more likely kill it)
    - **_*NOT*_** for `onResume()` + `onPause()`

For more sample code, showing how to bind to a service, see the [RemoteService.java][R3] class in [ApiDemos][R4].


Lifecycle of a Bound Service
---

![Imgur](http://i.imgur.com/2IfP3Qh.png)


Dish of the day
---
![Imgur](http://i.imgur.com/dOVd8Uf.jpg)
#肉屋うたがわ #fukuoka


Reference
---
[Bound Services][R1]


[R1]: https://developer.android.com/guide/components/services.html#CreatingBoundService
[R2]: https://developer.android.com/reference/android/os/DeadObjectException.html
[R3]: https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/app/RemoteService.html
[R4]: https://developer.android.com/resources/samples/ApiDemos/index.html
[R5]: https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/app/LocalService.html
[R6]: https://developer.android.com/resources/samples/ApiDemos/src/com/example/android/apis/app/LocalServiceActivities.html


