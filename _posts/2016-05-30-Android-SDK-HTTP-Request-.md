---
layout: post
title:  "Android SDK: HTTP Request (Using Retrofit)"
date:   2016-05-30 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Choose which one?
---

Android HTTP Clients:

- HTTPClient (legacy)
- HttpURLConnection (to replace HTTPClient after Android 2.2)
- OkHttp (built by Square Open Source, which is the engine that powers HttpUrlConnection since Android 4.4. Beside, prior to Gingerbread, HttpUrlConnection was unreliable)
- Apache HTTP Client (deprecated, maintaining cost too high)

Framework:

- Volley [(Transmitting Network Data Using Volley)][R1]
- Retrofit
- android-async-http

OkHttp and HttpUrlConnection are low-level HTTP clients. Volley is a high-level client that wraps OkHttp or HttpUrlConnection, it's not an HTTP client itself.

Use OkHttp. And if you use Volley, use OkHttp beneath it. Benefits are SPDY, HTTP/2, better connection re-use, more memory efficient, and the fact that it's actively developed.

Volley is official, while Retrofit is from Square project. Both of them are based on OkHttp, but you can change to another HTTP Client as you like(e.g., HttpURLConnection). As android-async-http is implemented based on HTTPClient, we do not consider android-async-http at this point. ["Android Async HTTP Clients: Volley vs Retrofit"][R4] saying that the performance of Retrofit is better than Volly, and we ended up picking Retrofit followed by ["okhttp,retrofit,android-async-http,volley应该选择哪一个？"][R3] suggested.


[Consuming APIs with Retrofit][R6]
---
(ongoing)


[Use Gson and Generate Models Manually][R7]
---
Gson is a library provides a framework for converting Java objects to JSON strings or vise versa. We can generate the Java model classes automatically or manually.

For example, a JSON string like:

```
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
We take this JSON string as our self-defined `LogWrapper` object. Inside the LogWrapper object, we have `api_tokens` and `logs` objects. Furthermore, the logs object has `type` and `attributes` objects. Finally, the attributes object has `date`, `name`, `message`, and `temperature`.

The objects here are just the Java model classes we have metioned here. So we can create our model like this:

LogWrapper.java

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
