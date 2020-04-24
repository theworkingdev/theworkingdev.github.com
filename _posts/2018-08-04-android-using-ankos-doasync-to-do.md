---
title: How to use Anko's doAsync for background task
layout: post
categories: android, kotlin
permalink: /2018/08/android-using-ankos-doasync-to-do.html 
date:   2018-08-01 
image: /images/book_las3_kotlin.png
---


I wrote a series of post on how to handle background processing in an Android app. You don’t really need to read them before you dive into this post, but it will give you context and more background about jank and how we use Threads, Handlers and AsyncTask to avoid it. The three previous post about jank are the following.

1. [Android Jank]({% post_url 2018-07-29-android-jank_31 %}). Running codes in the background using Java Threads
2. [Android Threads, Handlers and Messages]({% post_url 2018-07-28-android-threads-handlers-and-messages %}). Doing background work using Handlers and Messages, unlike Threads, Handler’s and Messages are part of the Android Framework (not part of Java)
2. [Android AsyncTask]({% post_url 2018-08-09-android-asynctask %}). Doing work in the background using the AsyncTask class. The AsyncTask, like Handlers and Messages, are also part of the Android Framework

In this post, we’ll take a look at another way to avoid jank. If you haven’t read the three previous post about jank, the following list summarizes what we’re trying to do.

What is jank?. When you try to do too much on the UI Thread, the Android runtime will start dropping frames. When your app’s FPS drops, the UI will stutter, it will be sluggish and annoying to use. This is jank
How do we avoid it?. Don’t try to do too much on UI Thread. Don’t;
Read from a large file, or write a large amount of info to a file
Connect to the network and read from it (or write)
Compute a complex routine Do these things in background thread
What is the UI Thread. It’s the original Thread that’s responsible for creating (and modifying) View elements in your app. Some developers refer to UI Thread as the "Main Thread"
What is a background thread. Any thread that isn’t the UI Thread. You generally create a background thread for your app


Now that you’re all caught up, let’s look at another way to run codes in the background. In this post, we will use the Anko library.

Anko is an Android library written in Kotlin by Jetbrains (the same company that created Kotlin). You can use for wide variety of task but for our purpose, we only need the doAsync portion. As it’s name implies, Anko’s doAsync will let us run codes asynchronously or in the background.

To start using Anko in a project, we need to add it’s dependency to the Gradle file.

```groovy
dependencies {
  ....
  implementation 'org.jetbrains.anko:anko-common:0.9'
}
```

The doAsync’s code structure is as follows

```kotlin
doAsync {
  // do things in the background  1
}
```

**(1)** In here, you can read or write to large files, download a file from the internet or do a task that will take a long time to complete. This block will execute in a background thread

The next challenge is how to go back to the UI Thread. Remember that a background thread is not allowed to change anything in the UI. Anko’s approach is is probably the simplest of all the other options (Threads, Handlers or AsyncTask).

```kotlin
doAsync {
  // do things in the background  // (1)
  activityUiThread {
    // make changes to the UI     // (2)
    textView.text = "Hello"
  }
}
```

**(1)** Background processing
**(2)** Now, you’re back to the UI Thread. It’s that simple. Whenever you need to go back to the UI Thread, just do it inside the activityUiThread block
The following code example shows a complete listing of a sample Activity which uses Anko’s doAsync to perform a long computation and then write something back to the UI
MainActivity, annotated code.

```kotlin
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import kotlinx.android.synthetic.main.activity_main.*
import org.jetbrains.anko.activityUiThread
import org.jetbrains.anko.doAsync

class MainActivity : AppCompatActivity() {

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    button.setOnClickListener {             // (1)
      doAsync {
        for(i in 1..15) {                   // (2)
          Thread.sleep(2000)                // (3)
          activityUiThread {
            textView.text = i.toString()    // (4)
          }
        }
      }
    }
  }
}
```

**(1)** Let’s setup a basic OnClickListener. This will trigger the background task

**(2)** Let’s just count from 1 to 15

**(3)** This simulates a long running task, our loop will come around 15 times, so the task will take a total of 30 seconds to complete

**(4)** Let’s tell the user what’s going on with the app. Update the TextView object with the current value of i

 {% include book_las3_kotlin.html %}