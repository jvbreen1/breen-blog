---
title: "Current Trends in Android Development"
date: 2018-04-11T00:00:00.000Z
description: >-
  Kotlin, Kotlin, Kotlin
---

Reposted from [https://engineering.upside.com/current-trends-in-android-development-39e073636118]

## Kotlin
[Kotlin, Kotlin, Kotlin](https://kotlinlang.org/). It’s the new hotness in Android, and we’re writing it at Upside. Kotlin brings a ton of modern language features to the Android ecosystem, including type inference, higher-order functions, optionals/null safety, and several quality of life improvements for interacting with Android APIs.

Combined with Google’s official support for Kotlin in Android, it’s no surprise, then, that the Android development community is trending toward Kotlin. At the start of KotlinConf 2017 in November, Google announced that “more than 17% of the projects in Android Studio 3.0 are now using Kotlin.” Keep in mind that official first-class support for Kotlin in Android was just announced in May 2017.

Kotlin is a blast to write, so look for more developers and companies to make the switch in 2018!
Kotlin is the new hotness in Android.
## App Development Platforms (Hi, Firebase!)

App development platforms, such as Firebase, have grown tremendously in popularity. Rather than rely on single-use SDKs, companies have increasingly turned to package solutions to provide a whole suite of functionality.

In the fast-paced startup world, we rely on solutions like these so that we can focus on our core product. The products these companies offer (A/B testing, analytics, metrics, etc.) are all vital to building successful apps, and the support of companies like Google, Microsoft, and Facebook means that we can be confident in what we’re integrating.

As these product suites expand, we’ll see people leaning on them to build better apps.
## Android Architecture Components

The Android Architecture Components modules are officially stable! This means we can use them in our apps to better work with the Android APIs and build cool features.

The main architecture components are:

    Lifecycle-aware components
    LiveData
    ViewModel
    Room

Lifecycle-aware components allow us to build features that automatically respond to Android lifecycle events. For example, let’s say you have a listener that needs to connect and disconnect when your Activity starts and stops. These new components can handle the lifecycle for you and reduce the amount of boilerplate code you write.

LiveData follows the observer pattern and allows you to build reactive UIs that update when your data updates. LiveData is lifecycle-aware as well, so it won’t update your UI when it is not active. This helps to prevent crashes, memory leaks, and stale data.

ViewModel is a data class that stores UI-related data. If you’ve heard of MVVM, this class will be familiar to you. Android’s implementation is lifecycle-aware and preserves your data upon configuration changes. If your screen rotates, your ViewModel will still be there. No more querying the API every time the screen rotates!

Room is a persistence library that sits on top of SQLite. It allows you to build Data Access Objects (DAOs) with interfaces and annotations, reducing boilerplate while still providing compile-time checking of fields and data types. You can use the full power of SQLite without going down into the weeds of SQLiteOpenHelper and cursors

It’s an exciting time to be an Android developer. We at Upside can’t wait to continue integrating these new components and features into our products and write some really cool code.
