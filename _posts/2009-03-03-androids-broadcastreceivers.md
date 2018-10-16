---
title: Android's BroadcastReceivers
layout: post
categories: android
permalink: /2009/03/androids-broadcastreceivers.html
date:   2009-03-01 
---

 BroadcastReceivers are one of the four Android components (the other three are Activities, Services and ContentProviders). A BroadcastReceiver is something that waits for a specific Intent to be broadcasted, and then it acts on it.

To create a simple broadcast receiver, you need to create a class that extends android.content.BroadcastReceiver; like this

```java
public class MyReceiver extends BroadcastReceiver {
  @Override
  public void onReceive(Context context, Intent intent) {
    Log.d("MyReceiver", "I got it");
    Toast.makeText(context, "I got it", Toast.LENGTH_LONG).show();
  }
} 
```

This class, by itself, won't do much unless we make it listen to a specific Intent. One of the ways to do that is to tell the Android runtime that want to listen to a specific Intent. We can do that by adding an intent-filter to the project's manifest file.; like this

```xml
<receiver android:name=".MyReceiver">
  <intent-filter>
    <action android:name="com.workingdev.DOSOMETHING"/>
  </intent-filter>
</receiver>
```

This intent-filter says that if an Intent like "com.workingdev.DOSOMETHING" is broadcasted, we'd like to respond to it.

Run the application. The application does not need to be visible for it to receive the broadcast

Send a broadcast message. There are two ways to send a broadcast, programmatically and via the adb shell. To send the message using adb, use the command from a terminal window

```bash
adb shell am broadcast -a com.workingdev.DOSOMETHING
```