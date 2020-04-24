---
title: Android Jank
layout: post
categories: android, kotlin
permalink: /2018/07/android-jank_31.html
date:   2018-07-29 
---

The following code used to trigger **ANR** (Android Not Responding) error

_Listing 1. A time consuming task._
```kotlin  
class MainActivity : AppCompatActivity() {

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    button.setOnClickListener {
      killSomeTime()
    }
  }

  private fun killSomeTime() {
    for (i in 1..20) {
      textView.text = i.toString()
      println("i:$i")
      Thread.sleep(2000)
    }
  }
}
```

But now, it doesn’t anymore. It’s still silly and sluggish though, but it won’t draw out the ANR error. Starting with Android 4.1 (Jelly Bean), Google introduced "Project Butter". Instead of throwing ANR, it will drop the framerates instead. An app, at Project Butter’s baseline, is supposed to run at 60FPS. If you try "to do too much work on the main thread", the runtime will start dropping framerates.

The sample code above won’t draw out an ANR error, but if you run it, you will notice the following;

1. You expect that the TextView will refresh every 2 seconds to show the current value of i. It won’t. The frames are gonna drop, so you won’t see any UI activity
2. But you will see the the value of i as it gets updated every 2 seconds in the Logcat window. This is because println isn’t using the UI Thread, so it’s not affected by the dropped frames
3. You might see a message like this from the runtime’s Choreographer
```
07-31 15:51:29.646 13403-13403/net.workingdev.ch15scratchasynctask
I/Choreographer: Skipped 2402 frames!  The application may be doing too 
much work on its main thread
```

The **killSomeTime** function in Listing 1 is representative of of programming task that can slow down the UI — jank. Generally, these are things like the following

1. Reading something from the network e.g. downloading an Image, downloading a web page etc.
2. Reading a large file from storage
3. Any sort of complex calculation

To avoid jank, make sure you don’t do any of these in the UI Thead. Do the janky things in a background thread. There’s plenty of ways to this in Android, one of them is to use the Thread class from Java. Listing 2 shows how do to do this.

_Listing 2. Using Threads to avoid Jank. Annotated code listing._
```kotlin
import android.os.AsyncTask
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    button.setOnClickListener {
      Thread(Runnable {                // (1)
        killSomeTime()                 // (2)
      }).start()                       // (3)
    }
  }

  private fun killSomeTime() {
    for (i in 1..20) {
      runOnUiThread(Runnable{          // (4)
        textView.text = i.toString()
      })
      println("i:$i")
      Thread.sleep(2000)
    }
  }
}
```

**(1)** To create a background thread, you need to create an instance of Runnable type (Thread) and start it. The Thread constructor takes a Runnable type and executes whatever is inside the run method. I used an object expression in this line to create an instance of a Runnable type without creating a named subclass — kinda like Java’s anonymous classes

**(2)** We are inside the Runnable’s run method now. The Runnable type is a SAM type (single abstract method), so you don’t have to write the run method explicitly, but we are inside it. Now we’re no longer in the UI thread, we’re in a background thread

**(3)** Don’t forget to call start on the Thread object

**(4)** One of the limitations of a background thread is that it cannot do anything that modifies the UI. Any UI modification code has to run from the original thread that created the UI — which is the UI Thread. If you need to change the UI from a background thread (like this) you can call the runOnUiThread method of the Activity class. It takes a Runnable type (again), you can put all the UI modification code on the run method of this Runnable type
 
{% include book_las3_kotlin.html %}