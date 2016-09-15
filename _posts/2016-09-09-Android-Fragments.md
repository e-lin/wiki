---
layout: post
title:  "Android Fragments"
date:   2016-09-09 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->

Overview
---
A Fragment represents a behavior or a portion of user interface in an Activity. You can combine multiple fragments in a single activity to build a multi-pane UI and reuse a fragment in multiple activities. You can think of a fragment as a modular section of an activity, which has its own lifecycle, receives its own input events, and which you can add or remove while the activity is running (sort of like a "sub activity" that you can reuse in different activities).

A fragment must always be embedded in an activity and the fragment's lifecycle is directly affected by the host activity's lifecycle. For example, when the activity is paused, so are all fragments in it, and when the activity is destroyed, so are all fragments. However, while an activity is running (it is in the resumed lifecycle state), you can manipulate each fragment independently, such as add or remove them.

An example of how two UI modules defined by fragments can be combined into one activity for a tablet design, but separated for a handset design:

![Imgur](http://i.imgur.com/vvnd1M6.png)

For example—to continue with the news application example—the application can embed two fragments in Activity A, when running on a tablet-sized device. However, on a handset-sized screen, there's not enough room for both fragments, so Activity A includes only the fragment for the list of articles, and when the user selects an article, it starts Activity B, which includes the second fragment to read the article. Thus, the application supports both tablets and handsets by reusing fragments in different combinations.


Lifecycle of a fragment (while its activity is running)
---
![Imgur](http://i.imgur.com/6vlL4QQ.png)


Creating a Fragment
---

### 1. Adding a user interface

### 2. Adding a fragment to an activity
Two ways you can add a fragment to the activity layout:

- Declare the fragment `<fragment/>` inside the activity's layout file.
- Or, programmatically add the fragment to an existing ViewGroup.

programmatically add the fragment like:

``` java
```

>The examples above show how to add a fragment to your activity in order to provide a UI. However, you can also use a fragment to provide a background behavior for the activity without presenting additional UI.


Dish of the Day
---
![Imgur](http://i.imgur.com/jEY34j2.jpg)
#homemade #tacoyaki


Reference
---
[Android Fragments][R1]<br />



[R1]: https://developer.android.com/guide/components/fragments.html#Creating
