---
title: "Using Singletons in Android"
date: 2016-03-20T00:00:00.000Z
description: >-
  When to use singletons in Android
---

Singletons can be very useful. There are many situations where it makes sense for your application to have a single, global reference to a piece of functionality. For example, your application may have some global configuration that is loaded via the Internet upon startup. It makes sense to only store that configuration (or a reference to it) in one place, and to only initialize it once.

In Java, this is relatively simple:

```
public class SingletonWithLoad
{
  
  private static SingletonWithLoad INSTANCE;
  
  private SingletonWithLoad()
  {
  }
  
  public static SingletonWithLoad getInstance()
  {
    if( INSTANCE == null )
      INSTANCE = new SingletonWithLoad();
      
    return INSTANCE;
  }
  
  public boolean isLoaded()
  {
    //some logic to determine if we've already loaded the information from the URL
  }
  
  public void load( String configUrl )
  {
    //some fancy URL Loading logic here
  }

}
```

We would then get and use our Singleton as follows:

```
Singleton mySingleton = Singleton.getInstance();
mySingleton.doThings();
```

In most Java applications, we can expect INSTANCE inside Singleton to exist for the life of the application. The same is true for Android, with one notable difference. If your application is running in the background (e.g. if you have touched the home button or opened another app), the Operating System may clean up your application if it needs the memory or processing resources your application is using.

# Android savedInstanceState

Android has built-in methods of handling application state in this case. For example, if your activity is about to be closed and garbage collected, the onSaveInstanceState method is called. It is in this method that you are expected to save off the activity’s custom state — which radio button is selected, the current value of a private instance variable, etc. — in order for it to be restored later. If the application is reopened later, then you can access the variables that you stored off in the savedInstanceState.

# Back to the Singleton

Since our Singleton is not attached to an Activity or Fragment, it does not always make sense for it to be bundled as part of the savedInstanceState when the application is stopped. A consequence of this fact is that the Singleton’s INSTANCE will be null when we reopen the app, and it will have to be recreated.

If the Singleton class knows everything it needs in order to construct itself, this is not necessarily a problem. When we call getInstance(), the Singleton will recreate its INSTANCE, and everything is fine.

However, if the Singleton needs to be parameterized when it is constructed, we may have a problem. For example, suppose the Singleton holds some configuration information that we pull down from a server. The Singleton will need to know the URL from which to pull that configuration. We may pass this URL into a load() method:

# Singleton with load() method

Then, for instance (pun somewhat intended), we would call load() in our Main Activity:

```
if( !ConfigFile.getInstance().isLoaded() )
  ConfigFile.getInstance().load( "http://example.com/config.json");
```

Our Singleton is now loaded from the URL. However, once our app is closed by Android and we navigate back to it, the Singleton’s INSTANCE may not be configured with the information from the URL. If we were in a different Activity (i.e. not the Main one) when we minimized the app, the load() method will not be called upon resuming the app. If this second activity relies on the Singleton being configured properly, we will get errors.

# So what now?

There are a few solutions to this error, and which solution you choose depends on the needs and usage of your Singleton class.

# SavedInstanceState (again)

In your activities’ onSaveInstanceState method, you can save a copy of your Singleton’s INSTANCE via getInstance(). This works well if your configuration doesn’t change often and can be easily serialized (e.g. JSON, XML, etc.). The main drawback to this approach is that it may result in a Singleton with stale configuration. If the user leaves the app in the background and the configuration changes on the server, then your saved copy of the INSTANCE will be out of date.

# Reloading the Singleton in the Activity

If the configuration of the Singleton changes often, then you will probably want to reload it when your app comes back into the foreground after being killed. You can do this by adding the load() logic into your other Activities, thus making sure the config is reloaded when any Activity is loaded. There is one major caveat to this approach.

Network operations in Android must be performed on a background (read: not the UI) thread. This makes sense, since it’s a bad User Experience to block the UI thread while waiting for I/O that may take a while. However, what this practically means is that the UI will continue to reinitialize in your activities while the configuration for the Singleton loads in the background. If the configuration doesn’t load almost instantly, parts of the Activity or any of its Fragments that require a configured Singleton will fail.

A partial solution would be to verify that your app’s views are fault-tolerant and fall back to default cases when the Singleton is not yet configured. In addition, you could add callbacks into the Singleton so that your Activities and Fragments could listen to the Singleton and be notified when it is configured. This is similar to the pattern used in AsyncTask, so it’s probably good practice to follow.

# Loading Configuration in a Service

Depending on the needs of your application, you may choose to load your Singleton’s configuration in a Service. This provides the benefit of automatically downloading the configuration in the background, ensuring a properly configured Singleton at all times. Services run in the background, performing tasks such as network I/O, scheduling, etc.

I have not personally utilized services very extensively, so I won’t pretend to know everything about them. However, one general word of caution would be to make sure you are mindful of Network and Data usage in the background. If your configuration is very large and requires a lot of Network I/O, it may consume a lot of the user’s limited data in the background. Some phones are set to disable wifi when the screen is off, so your app presumably would be downloading over cellular data.

It might be worthwhile to add a setting to your app that only allows downloading over Wi-Fi. It also may make sense to implement a quicker, smaller API to determine if your configuration file has changed. This way, you would only download the whole file if it has changed.

# Conclusion

I hope this has been useful. It is by no means a complete guide on Singletons, the Activity/Fragment lifecycle, or Services, but I have wrestled with Singletons enough to hopefully provide some insight into a somewhat common pitfall of Android development. Please feel free to add a comment if you have additional ideas or if I’ve written something that’s inaccurate.

If you liked this article, you can follow me here on Medium or follow my open-source commits on GitHub.