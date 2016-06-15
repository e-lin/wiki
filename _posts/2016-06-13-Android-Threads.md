---
layout: post
title:  "Android Threads"
date:   2016-06-13 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Threads
---
Simply two rules to Android's single thread model:

- Do not block the UI thread(main thread)
- Do not access the Android UI toolkit(components from `android.widget` and `android.view` packages.) from outside the UI thread

#### Worker threads (Background threads)

An example of a worker thread that violates the second rule of the single-threaded model:

``` java
public void onClick(View v) {
    new Thread(new Runnable() {
        public void run() {
            Bitmap b = loadImageFromNetwork("http://example.com/image.png");
            mImageView.setImageBitmap(b);
        }
    }).start();
}
```

Solve by one of below three methods:

- Activity.runOnUiThread(Runnable)
- View.post(Runnable)
- View.postDelayed(Runnable, long)

``` java
public void onClick(View v) {
    new Thread(new Runnable() {
        public void run() {
            final Bitmap bitmap =
                    loadImageFromNetwork("http://example.com/image.png");
            mImageView.post(new Runnable() {
                public void run() {
                    mImageView.setImageBitmap(bitmap);
                }
            });
        }
    }).start();
}
```

To handle more complex interactions with a worker thread, consider below solutions in your worker thread, to process message delivered from the UI thread.

- Handler and HandlerThread
- AsyncTask

#### Using AsyncTask

``` java
public void onClick(View v) {
    new DownloadImageTask().execute("http://example.com/image.png");
}

private class DownloadImageTask extends AsyncTask<String, Void, Bitmap> {
    /** The system calls this to perform work in a worker thread and
      * delivers it the parameters given to AsyncTask.execute() */
    protected Bitmap doInBackground(String... urls) {
        return loadImageFromNetwork(urls[0]);
    }

    /** The system calls this to perform work in the UI thread and delivers
      * the result from doInBackground() */
    protected void onPostExecute(Bitmap result) {
        mImageView.setImageBitmap(result);
    }
}
```

See Also: [Communicating with the UI Thread][R2].

#### Stop the running thread in the onStop() of your activity

If you are using the Thread in the Activity for some purpose, it is good practice to stop it when your activity destroy.

``` java
class DBThread extends Thread {
    @Override
    public void run() {
        while (!isInterrupted()) {
            //do something
        }
    }
}

@Override
public void onStop() {
    if (null != myThread) {
        myThread.interrupt(); // request to terminate thread in regular way

        myThread.join(500); // wait until thread ends or timeout after 0.5 second

        if (myThread.isAlive()) {
            // this is needed only when something is wrong with thread, for example hangs in ininitive loop or waits to long for lock to be released by other thread.
            Log.e(TAG, "Serious problem with thread!");
            myThread.stop();
        }
    }
    super.onStop();
}
```
Note that `Thread.destroy()` is deprecated and will not work. `Thread.stop()` is also deprecated since it can brake state of virtual machine (not thread-safey), so it should be use only as last resort and this is what my code do and that is why my code logs an error.

See Also: [Activitys, Threads, & Memory Leaks][R5].

#### Cancel the AsyncTask in the onStop() of your activity

When starting:

``` java
Task myTask = new Task();
myTask.execute();
```
then in onPause()/onStop():

``` java
if (null != myTask) {
    myTask.cancel(true);
}
```
then inside your doInBackground() if you are doing something periodically (like downloading) have something like this:

``` java
if (isCanceled()) {
    return;
}
```
See Also: [AsyncTask won't stop even when the activity has destroyed][R7]

#### AsyncTask Status
using `getStatus()` to get current state:

- PENDING: AsyncTask has been instantiated but `execute()` hansn’t been called on the instance.
- RUNNING: `execute()` has been called.
- FINISHED: The execution has been done as well as `onPostExecute()` or `onCancelled()` has been called.

#### Should you use a AsyncTask or a thread?
For long-running or CPU-intensive tasks, there are basically two ways to do this: Java threads, and Android's native AsyncTask.

Neither one is necessarily better than the other, but knowing when to use each call is essential to leveraging the system's performance to your benefit.

*Use AsyncTask for:*

- Simple network operations which do not require downloading a lot of data
- Disk-bound tasks that might take more than a few milliseconds

*Use Java threads for:*

- Network operations which involve moderate to large amounts of data (either uploading or downloading)
- High-CPU tasks which need to be run in the background
- Any task where you want to control the CPU usage relative to the GUI thread

See Also: [Android background processing with Handlers, AsyncTask and Loaders - Tutorial][R10]


Threads vs Services
---
There is never a choice between using a Thread and using a Service, as they are distinct questions which must be answered independently.

*Services* is for to run your task independently of the `Activity`, it allows you to run any task in background. This run on the main UI thread so when you want to perform any network or heavy load operation then you have to use the `Thread` there. Suppose you want to take backup of your instant messages daily in the background then here you would use the `Service`.

*Threads* is for run your task in its own thread instead of main UI thread. You would use when you want to do some heavy network operation like sending bytes to the server continuous and it is associated with the android components. When your component destroy who started this then you should have stop it also.


Should you use a service or a thread?
---
A service is simply a component that can run in the background even when the user is not interacting with your application. Thus, you should create a service only if that is what you need.

If you need to perform work outside your main thread, but only while the user is interacting with your application, then you should probably instead create a new thread and not a service. For example, if you want to play some music, but only while your activity is running, you might create a thread in onCreate(), start running it in onStart(), then stop it in onStop(). Also consider using AsyncTask or HandlerThread, instead of the traditional Thread class. See the Processes and Threading document for more information about threads.

Remember that if you do use a service, it still runs in your application's main thread by default, so you should still create a new thread within the service if it performs intensive or blocking operations.


Reference
---
[Android Processes and Threads][R1]<br />
[Thread still runnng after destroying the the Activity][R3]<br />
[Service vs Thread in Android][R4]<br />
[Android Services][R6]<br />
[Understanding Android AsyncTask (Executing Background Tasks)][R8]<br />
[Asynctask vs Thread in android][R9]


[R1]: https://developer.android.com/guide/components/processes-and-threads.html
[R2]: https://developer.android.com/training/multiple-threads/communicate-ui.html
[R3]: http://stackoverflow.com/questions/11536625/thread-still-runnng-after-destroying-the-the-activity
[R4]: http://stackoverflow.com/questions/22933762/service-vs-thread-in-android
[R5]: http://www.androiddesignpatterns.com/2013/04/activitys-threads-memory-leaks.html
[R6]: https://developer.android.com/guide/components/services.html
[R7]: http://stackoverflow.com/questions/2531336/asynctask-wont-stop-even-when-the-activity-has-destroyed
[R8]: http://codetheory.in/android-asynctask/
[R9]: http://stackoverflow.com/questions/18480206/asynctask-vs-thread-in-android
[R10]: http://www.vogella.com/tutorials/AndroidBackgroundProcessing/article.html

