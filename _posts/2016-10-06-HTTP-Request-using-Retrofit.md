---
layout: post
title:  "HTTP Request using Retrofit"
date:   2016-10-06 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Choose which one?
---

Android HTTP Clients (Low level):

- **HTTPClient** (legacy)
- **HttpURLConnection** (to replace HTTPClient after Android 2.2)
- **OkHttp** (built by Square Open Source, which is the engine that powers HttpUrlConnection since Android 4.4.)
- **Apache HTTP Client** (deprecated, maintaining cost too high)

Framework (High level):

- **Volley** [(Transmitting Network Data Using Volley)][R1]
- **Retrofit**
- **android-async-http**

OkHttp and HttpUrlConnection are low-level HTTP clients. Volley is a high-level client that wraps OkHttp or HttpUrlConnection, it's not a HTTP client itself.

Use OkHttp. And if you use Volley, use OkHttp beneath it. Benefits are SPDY, HTTP/2, better connection re-use, more memory efficient, and the fact that it's actively developed.

Volley is official, while Retrofit is from Square project. Both of them are based on OkHttp, but you can change to another HTTP Client as you like(e.g., HttpURLConnection).

As android-async-http is implemented based on HTTPClient, we do not consider android-async-http at this point.

Furthermore, >["Android Async HTTP Clients: Volley vs Retrofit"][R4] says that the performance of Retrofit is better than Volly. Basically it is all right to choose both Volley or Retrofit, but I ended up picking Retrofit followed by >["okhttp,retrofit,android-async-http,volley应该选择哪一个？"][R3] suggested.


Let's start to use Retrofit
---

### 1. Setup
Make sure to require Internet permissions in your AndroidManifest.xml file:

``` xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-permission android:name="android.permission.INTERNET" />
</manifest>
```
Add the following to your app/build.gradle file:

``` gradle
dependencies {
    compile 'com.google.code.gson:gson:2.6.2'
    compile 'com.squareup.retrofit2:retrofit:2.1.0'
    compile 'com.squareup.retrofit2:converter-gson:2.1.0'
}
```

### 2. Create Java Classes for Resources

There is another automated approach to auto-generating the Java classes. However, this time I would like to use Gson to generate models manually.

A defined JSON string like:

``` json
{
"api_token": "************************",
"logs": [
    {
    "type": "message",
    "attributes": {
        "date": 1467262800,
        "name": "device_001",
        "message": "hello"
        }
    },
    {
    "type": "temperature",
    "attributes": {
        "date": 1467266400,
        "name": "device_fukuoka_001",
        "temperature": 24.5
        }
    }
    ]
}
```

1. I make this JSON string to a self-defined `LogWrapper` object. Inside the LogWrapper object, I have `api_tokens` and `logs` objects.

2. Furthermore, the logs object has `type` and `attributes` objects.

3. Finally, the attributes object has `date`, `name`, `message`, and `temperature`.

The objects here are just the Java model classes we have metioned here. So we can create our model like this:

**LogWrapper.java**

``` java
public class LogWrapper {
    String api_token;
    List<Log> logs;

    public static class Log {
        String type;
        LogAttributes attributes;
    }

    public static class LogAttributes {
        long date;
        String name;
    }

    public static class LogMessageAttributes extends LogAttributes {
        String message;
    }

    public static class LogTemperatureAttributes extends LogAttributes {
        float temperature;
    }
}
```

### 3. Creating the Retrofit instance

To send out network requests to an API, use the [Retrofit builder][R8] class and specify the base URL for the service.

``` java
// Trailing slash is needed
public static final String BASE_URL = "http://api.myservice.com/";
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl(BASE_URL)
    .addConverterFactory(GsonConverterFactory.create())
    .build();
```

If you wish to pass in a custom Gson parser instance, it can be specified too:

``` java
Gson gson = new GsonBuilder()
        .setDateFormat("yyyy-MM-dd'T'HH:mm:ssZ")
        .create();

Retrofit retrofit = new Retrofit.Builder()
    .baseUrl(BASE_URL)
    .addConverterFactory(GsonConverterFactory.create(gson))
    .build();
```

### 4. Define the Endpoints

With Retrofit 2, endpoints are defined inside of an interface using special retrofit annotations to encode details about the parameters and request method. In addition, the return value is always a parameterized `Call<T>` object such as `Call<User>`. If you do not need any type-specific response, you can specify return value as simply `Call<ResponseBody>`.

For instance, the interface defines each endpoint in the following way:

**IoTBoard.java**

``` java
public interface IoTBoard {

    @POST("./")
    Call<ResponseMessage> postLog(@Body LogWrapper log);
}
```

**ResponseMessage.java**

``` java
public class ResponseMessage {
    boolean success;
    String message;

    @Override
    public String toString() {
        return "Success: " + success + " Message: " + message;
    }
}
```

Then, create an implementation of the API endpoints defined by the `IoTBoard` interface.

``` java
IoTBoard iotBoard = retrofit.create(IoTBoard.class);
```

It ended up looking like:

![Imgur](http://i.imgur.com/6K2jlpK.png)

Prepare the log you want to _post_ to the `IoTBoard`

``` java

    LogWrapper lw = new LogWrapper();
    lw.api_token = BuildConfig.API_TOKEN;
    lw.logs = new ArrayList<LogWrapper.Log>();
    LogWrapper.Log log = new LogWrapper.Log();
    log.type = BuildConfig.TABLE_TYPE;
    log.attributes = new LogWrapper.LogAttributes();
    log.attributes.date = tsLong;
    log.attributes.id = data.getId();
    log.attributes.os = Build.VERSION.RELEASE;
    log.attributes.model = Build.MODEL;
    log.attributes.platform = Build.PRODUCT;
    TelephonyManager tm = (TelephonyManager)getActivity().getSystemService(Context.TELEPHONY_SERVICE);
    log.attributes.carrier = tm.getNetworkOperatorName();
    log.attributes.access_url = maker_url;
    lw.logs.add(log);
```

and send it like:

``` java

    final Call<ResponseMessage> caller = iotBoard.postLog(lw);

    caller.enqueue(new Callback<ResponseMessage>() {
        @Override
        public void onResponse(Call<ResponseMessage> call, Response<ResponseMessage> response) {
            if (null != response.body()){
                Log.d("IoTBoard", response.body().toString());
            }
        }

        @Override
        public void onFailure(Call<ResponseMessage> call, Throwable t) {
            Log.d("IoTBoard", "Something went wrong: " + t.getMessage());
        }
    });
```

Note also that OkHttp, which dispatches the callback on the worker thread, callbacks in Retrofit are dispatched on the [main thread][R9]. Because UI updates can only be done on the main thread, the approach used by Retrofit can make it easier to make changes to your views.

If you are using Retrofit in a [Background Service][R10] instead of an Activity or Fragment, you can run the network call synchronously within the same thread by using the execute() method.

``` java
try {
  Response<User> response = call.execute();
} catch (IOException e ){
   // handle error
}
```


Dish of the Day
---

![Imgur](http://i.imgur.com/I9mhlhy.jpg)
#homemade #asparagus #bacon


Reference
---
[Why should I use OkHttp instead of android httpClient and AsyncTask][R2]<br />
[okhttp,retrofit,android-async-http,volley应该选择哪一个？][R3]<br />
[Android Async HTTP Clients: Volley vs Retrofit][R4]<br />
[Glide 3: okhttp vs Volley vs HttpUrlConnection][R5]<br />
[Consuming APIs with Retrofit][R6]<br />
[Leveraging the Gson Library][R7]



[R1]: https://developer.android.com/training/volley/index.html
[R2]: http://stackoverflow.com/questions/25739620/why-should-i-use-okhttp-instead-of-android-httpclient-and-asynctask
[R3]: https://www.zhihu.com/question/35189851
[R4]: http://instructure.github.io/blog/2013/12/09/volley-vs-retrofit/
[R5]: https://www.reddit.com/r/androiddev/comments/2fmul3/glide_3_okhttp_vs_volley_vs_httpurlconnection/
[R6]: https://github.com/codepath/android_guides/wiki/Consuming-APIs-with-Retrofit
[R7]: https://github.com/codepath/android_guides/wiki/Leveraging-the-Gson-Library
[R8]: http://square.github.io/retrofit/2.x/retrofit/retrofit2/Retrofit.Builder.html
[R9]: http://stackoverflow.com/questions/21006807/does-retrofit-make-network-calls-on-main-thread/21010181#21010181
[R10]: https://github.com/codepath/android_guides/wiki/Starting-Background-Services

