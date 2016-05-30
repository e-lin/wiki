---
layout: post
title:  "Android SDK: HTTP Request"
date:   2016-05-30 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Android HTTP Clients:

- HTTPClient
- OkHttp
- Apache HTTP Client
- HttpURLConnection

Framework:

- Volley [(Transmitting Network Data Using Volley)][R1]
- Retrofit
- android-async-http

According to ["Why should I use OkHttp instead of android httpClient and AsyncTask"][R2], HttpClient is now a legacy. We better use OkHttp since Android SDK 23 Marshmallow.

Volley is official, while Retrofit is from Square project. Both of them are based on OkHttp, but you can change to another HTTP Client as you like. As android-async-http is implemented based on HTTPClient, we do not consider android-async-http at this point. ["Android Async HTTP Clients: Volley vs Retrofit"][R4] saying that the performance of Retrofit is better than Volly, and we ended up picking Retrofit followed by ["okhttp,retrofit,android-async-http,volley应该选择哪一个？"][R3] suggested.



Reference
---
[Why should I use OkHttp instead of android httpClient and AsyncTask][R2]<br />
[okhttp,retrofit,android-async-http,volley应该选择哪一个？][R3]<br />
[Android Async HTTP Clients: Volley vs Retrofit][R4]

[R1]: https://developer.android.com/training/volley/index.html
[R2]: http://stackoverflow.com/questions/25739620/why-should-i-use-okhttp-instead-of-android-httpclient-and-asynctask
[R3]: https://www.zhihu.com/question/35189851
[R4]: http://instructure.github.io/blog/2013/12/09/volley-vs-retrofit/