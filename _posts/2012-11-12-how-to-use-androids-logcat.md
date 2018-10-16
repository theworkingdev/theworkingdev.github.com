---
title: How to use Android's Logcat
layout: post
categories: android
permalink: /2012/11/how-to-use-androids-logcat.html
date:   2012-11-12 
---

This section is not about some spiffy tool that lets put breakpoints in your IDE or something that allows you to step through your code line-by-line. Nothing like that. This is about how to use a crude and old technique of debugging. Outputting something into your screen at specific points in the code.

In Core Java, you can simply use the System.outprintln(), it works a little differently in Android.

You can still write System.out.println in your code but it won't show up on the Android screen. The target of println is the stdout, hence it will show up on your android debugger session (adb) and not the Android device or emulator. Also, instead of using println, use the Log class instead if you need to insert some debugging info on your app.

A basic use of the Log class is as shown below.

```java
import android.util.Log;

public class MyAndroidApp extends Activity {

  private final String TAG = getClass().getName();
    ....
    
    public void someCallBack() {
      Log.v(TAG, "string to log");
    }
    
    public void someMethod() {
      Log.v(TAG, "string to log");
    }
}
```

On a terminal window, run `adb logcat`. The logcat utility supports a couple of flags. Here's how you might use them

* `adb logcat` — Prints everything that happens to the device, on the screen. You probably don’t want this
* `adb logcat * -S` — Direct opposite of adb logcat, this is very silent, you won’t see anything (meaningful)
* `adb logcat -s `— filters the output messages that matches the TAG name

Okay. Now lets try them on using the callbacks of the Activity class.

```java
package com.thelogbox;

import android.app.Activity;
import android.content.Context;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.LinearLayout;

class LCycleView extends LinearLayout implements View.OnClickListener {

  LCycleView(Context ctx) {
    super(ctx);
    setOnClickListener(this);
  }

  public void onClick(View view) {
    System.out.println("Clicked");
  }
}

public class LCycle extends Activity {

  /** Called when the activity is first created. */

  String TAG = getClass().getName();

  @Override
    public void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(new LCycleView(this));

      Log.v(TAG, "onCreate");
    }

  @Override
  public void onResume() {
    super.onResume();
    Log.v(TAG, "onResume");
  }    

  @Override
  public void onRestart() {
    super.onRestart();
    Log.v(TAG, "Restart");
  }

  @Override
  public void onPause() {
    super.onPause();
    Log.v(TAG, "Paused");
  }

  @Override
  public void onStop() {
    super.onStop();
    Log.v(TAG, "Stoppped");
  }

  @Override 
  public void onDestroy(){
    super.onDestroy();
    Log.v(TAG, "Destroy");
  }
}
```

The above code is one of the few examples an Android developer might spike while beginning to explore Android programming. After reading all about lifecycle of the Activity class, of course you want to know when the events will actually be fired.

If you simply type adb logcat, everything that an application does (and the operating system) will be displayed on the screen, it will also scroll very fast because there are lots of things that the Android operating system is doing.

If you want to see only the logs associated with your app, type adb `logcat -s com.thelogbox.LCycle`
