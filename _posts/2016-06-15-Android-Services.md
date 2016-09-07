---
layout: post
title:  "Android Services"
date:   2016-06-15 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Overview
---
A Service is an application component that can perform long-running operations in the background and does not provide a user interface.

A service can essentially take two forms:

**Started:**

>A service is "started" when an application component (such as an activity) starts it by calling `startService()`. Once started, a service can run in the background indefinitely, even if the component that started it is destroyed. Usually, a started service performs a single operation and does not return a result to the caller. For example, it might download or upload a file over the network. When the operation is done, the service should stop itself.

**Bound:**

>A service is "bound" when an application component binds to it by calling `bindService()`. A bound service offers a client-server interface that allows components to interact with the service, send requests, get results, and even do so across processes with interprocess communication (IPC). A bound service runs only as long as another application component is bound to it. Multiple components can bind to the service at once, but when all of them unbind, the service is destroyed.

You can have a service started, bounded, or worked in both ways(started + bounded). Meanwhile, any application component can use the service (even from a separate application), you can declare the service as private, in the manifest file, and block access from other applications.


Remember this
---
A service runs in the main thread of its hosting process—the service **does not** create its own thread and **does not** run in a separate process (unless you specify otherwise). This means that, if your service is going to do any CPU intensive work or blocking operations (such as MP3 playback or networking), you should create a new thread within the service to do that work. By using a separate thread, you will reduce the risk of Application Not Responding (ANR) errors and the application's main thread can remain dedicated to user interaction with your activities.


Basics
---
To create a Service, you need:

- create a subclass of `Service` (or one of its existing subclasses).
- override some callback methods that handle key aspects of the service lifecycle
- provide a mechanism for components to bind to the service

The most important callback methods you should override are:

- onStartCommand()

    >The system calls this method when another component, such as an activity, requests that the service be started, by calling `startService()`. Once this method executes, the service is started and can run in the background indefinitely. If you implement this, it is your responsibility to stop the service when its work is done, by calling `stopSelf()` or `stopService()`. (If you only want to provide binding, you don't need to implement this method.)

- onBind()

    >The system calls this method when another component wants to bind with the service (such as to perform RPC), by calling `bindService()`. In your implementation of this method, you must provide an interface that clients use to communicate with the service, by returning an `IBinder`. You must always implement this method, but if you don't want to allow binding, then you should return null.

- onCreate()

    >The system calls this method when the service is first created, to perform one-time setup procedures (before it calls either `onStartCommand()` or `onBind()`). If the service is already running, this method is not called.

- onDestroy()

    >The system calls this method when the service is no longer used and is being destroyed. Your service should implement this to clean up any resources such as threads, registered listeners, receivers, etc. This is the last call the service receives.

Lifecycles
---

- If a component starts the service by calling `startService()` (which results in a call to `onStartCommand()`), then the service remains running until it stops itself with `stopSelf()` or another component stops it by calling `stopService()`.

- If a component calls `bindService()` to create the service (and `onStartCommand()` is not called), then the service runs only as long as the component is bound to it. Once the service is unbound from all clients, the system destroys it.

- The Android system will force-stop a service only when memory is low and it must recover system resources for the activity that has user focus. If the service is bound to an activity that has user focus, then it's less likely to be killed, and if the service is declared to `run in the foreground`, then it will almost never be killed. Otherwise, if the service was started and is long-running, then the system will lower its position in the list of background tasks over time and the service will become highly susceptible to killing—if your service is started, then you must design it to gracefully handle restarts by the system. If the system kills your service, it restarts it as soon as resources become available again (though this also depends on the value you return from `onStartCommand()`).


Creating a Started Service
---

**Service:**

- multiple requests simultaneously
- developers must create a new thread in which to do all the service's work

**IntentService:**

- simple
- uses a worker thread to handle all start requests, one at a time
- developers only need to do is implement `onHandleIntent()`


Creating a Bound Service
---
A bound service is one that allows application components to bind to it by calling `bindService()` in order to create a long-standing connection (and generally does not allow components to start it by calling `startService()`).

You should create a bound service when you want to interact with the service from activities and other components in your application or to expose some of your application's functionality to other applications, through interprocess communication (IPC).

There are multiple ways to implement a bound service and the implementation is more complicated than a started service, so the bound service discussion appears in a separate document about [Bound Services][R3].


Dish of the day
---
![Imgur](http://i.imgur.com/i6rCpsN.jpg)
#hungry_heaven #fukuoka


Reference
---
[Android Service][R1]<br />
[App to monitor other apps on android][R2]<br />
[Bound Services][R3]


[R1]: https://developer.android.com/guide/components/services.html
[R2]: http://stackoverflow.com/questions/20416610/app-to-monitor-other-apps-on-android
[R3]: https://developer.android.com/guide/components/services.html#CreatingBoundService

