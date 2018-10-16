---
title: Android Threads, Handlers and Messages
layout: post
categories: kotlin, android
permalink: /2018/07/android-threads-handlers-and-messages.html
date:   2018-07-28 
---

> If your app suffers from slow UI rendering, then the system is forced to skip frames and the user will perceive stuttering in your app. We call this jank.	
> -- developer.android.com

In a previous post, we started looking for ways to avoid jank. In that post, we used the trusty ole Threads from Java. In this post, we’ll look at another way to run codes in a background thread.
The main reason for using a background thread is to avoid tying up the UI thread. The UI thread is where all UI related actions happen e.g. creating views and updating their values/appearances. If you do anything time-consuming while on the UI thread, like, reading from network I/O, reading from a file or doing a complex calculation, the time that the Android runtime spends on those codes, is time not spent on updating the UI. Anytime that the runtime senses you’re "doing too much things on the UI thread", it will drop the framerates. You may not get the ANR error but the app will run sluggishly because the frames per second will drop.

The main limitation when using a background thread is that you cannot make any changes to the UI. When you need to update the UI, you need to find a way how to get back to the UI Thread. In the previous post, we used the runOnUiThread method of the Activity class to get back to the UI thread. In this post, we will use Handlers and Messages

A Handler is part of the Android Framework that is used for managing threads. Each thread has an associated MessageQueue and a Handler. Each instruction you write in your app ends up in a MessageQueue. So, if you thought that when you wrote textView.setText(), that the runtime immediately carried out your instruction, you would be wrong. That instruction would have been sent to a MessageQueue where it awaits until a Handler gets to it and executes it.

To use a Handler object in your app, you generally need to do the following;

Get the Handler object that’s associated with the UI Thread
Somewhere in your code, when you’re about to do something that may cause jank, run that instead on a background thread
While you’re inside the background thread, when you need to change something in the UI, do the following;
Create a Message object, best way to do this is to call Message.obtain()
Send a message to the Handler object by calling the sendMessage method. Message objects can carry data. The data attribute of the Message object is a Bundle object, so you can use the various putXXX() methods on it e.g. putString, putInt, putBundle, putFloat etc.
In the handleMessage callback of the Handler object, do your UI changes

Listing 1 shows how you can do this in code.

_Listing 1.Background task using Handlers and Messages._ 
```kotlin
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import android.os.Handler
import android.os.Message
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

  lateinit var mhandler: Handler                        // (1)

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    mhandler = object : Handler() {                     // (2)
      override fun handleMessage(msg: Message?) {
        textView.text = msg?.data?.getString("counter") // (3)
      }
    }

    button.setOnClickListener {
      Thread(Runnable {
        killSomeTime()                                  // (4)
      }).start()
    }
  }

  private fun killSomeTime() {
    for (i in 1..20) {
      var msg = Message.obtain()                        // (5)
      msg.data.putString("counter", i.toString())       // (6)
      mhandler.sendMessage(msg)                         // (7)
      Thread.sleep(2000)
    }
  }
}
```

**(1)** Declare a Handler object as a property of the class. We need access to this from two of our top level functions. We’re using lateinit here because were not yet ready to define the object

**(2)** We’re defining the Handler object now. Were getting the Handler object that’s associated with the UI Thread — we’re not creating a new Handler

**(3)** It’s safe to make UI changes in here. This is the Handler that’s associated with the UI Thread. The handleMessage callback will be called by the runtime when we invoke the sendMessage. The Message parameter of this method carries the data

**(4)** killSomeTime is representative of any I/O or time-consuming task. Always run it in a background thread to avoid jank

**(5)** Create a Message object. This is what we will send to the Handler object later

**(6)** The data property of the Message object is like a Bundle you can put things in it. It’s like a dictionary, each item is a pair --- a key and a value. We passed two things to the putString() method, these are;
  * "counter", the key 
  * `i.toString()`, the value

**(7)** Send the Message to the Handler object
 