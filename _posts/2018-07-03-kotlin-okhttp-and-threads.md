---
title: OkHTTP and Threads
layout: post
categories: kotlin, android
permalink: /2018/07/kotlin-okhttp-and-threads.html
date:   2018-07-03 
image: /images/android-many.jpg
description: Android has various ways to run your code in the background. From what I can recall, we can use Threads, Handers, Messages, AsyncTask and many others
---

Android has various ways to run your code in the background. From what I can recall, we can use any of the following;

* Good ole **Threads** and **Runnables**
* The newer AsyncTask and **AsyncTaskLoader**
* Services, these are for heavy duty tasks
* and the newest one, Anko’s **doAsync**

Each option has pros and cons and some approaches are more suitable to some problems. So you need to consider the fitness of each approach’s mechanics for the problem you’re trying to solve.But why do we need to bother? Why run some of our codes in the background? Because the Android runtime is a strict police of performance. It won’t let you hog resources for a long period of time. If your app hogs processing time and other computing resources, the runtime can kill your app and then your users will see the **ANR** error (Android Not Responding).

When a user clicks your app, the runtime launches it and it is given 1 thread to do everything it needs to do. This thread, famously known as the **UI thread** is where everything will happen. Whenever you do something like change the text attribute of a TextView, build a menu dynamically or respond to a click, all of these things will happen on the UI thread. So you can’t hog it. If you’re just doing some quick calculations and displaying the result to a TextView, you should’nt worry too much because the UI thread can handle without breaking a sweat. However, there are things you will do that isn’t as simple as displaying the result of a Math calculation; Sometimes you may need to read from a database, download a JSON file from the Internet or read the contents of a file. You know, I/O stuff. These things take time. These are the kinds of things you cannot do on the UI thread.


If we can’t do it on the UI thread, where are supposed to do it? In a background thread.


To dive into a bit of detail, let’s do a sample task. We’ll pull some user information from GitHub. There’s a publicly accessible endpoint at `https://api.github.com/users/UserName` (replace **UserName** with a valid Github login id).


Let’s use **OkHttp** for the http requirements; many developers are using this library nowadays.
We need to do 2 things before we can use OkHttp. We need to include the OkHttp dependency on our gradle script, then we need to add the INTERNET permission to project’s manifest file.

Add the OkHttp dependency on the gradle module. 

```gradle
dependencies {
  ...
  implementation 'com.squareup.okhttp:okhttp:2.5.0'
}
```

Add the INTERNET permission to the manifest. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="net.workingdev.httpscratch">
  <uses-permission android:name="android.permission.INTERNET"/> <!--(1)--> 
  <application
  ....
  </application>=
</manifest>
```

**(1)** You should add this to the project’s AndroidManifest file
Now we’re ready to start. We will use the following code snippet to pull the GitHub info from the web.

_Listing 1. OkHttp sample usage. _
```kotlin
val url = "https://api.github.com/users/tedhagos"
println("inside doGetHttp")
val client = OkHttpClient()
val request = Request.Builder().url(url).build()
val response = client.newCall(request).execute()

val bodystr = response.body().string()
```

If you run this code on the UI thread, say, inside an OnClickListener, like this

_Listing 2. OkHttp code inside a click listener._ 
```kotlin
button.setOnClickListener {
  val url = "https://api.github.com/users/tedhagos"
  println("inside doGetHttp")
  val client = OkHttpClient()
  val request = Request.Builder().url(url).build()
  val response = client.newCall(request).execute()

  val bodystr = response.body().string()
}
```

This will throw the **NetworkOnMainThreadException**. We’re not supposed run any code that accesses the network on the UI thread. So, we’ll run in a background thread.
Using Threads and Runnables

We can use the tried and tested **Thread** class to make this work. If we run all the network codes in background thread, the Android runtime shouldn’t give us anymore problems.The following listing shows an annotated MainActivity with an inner class that extends the **Runnable** interface. We run all our network codes inside the run method.

Listing 3. Annotated MainActivity.
```kotlin  
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import com.squareup.okhttp.OkHttpClient
import com.squareup.okhttp.Request
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    button.setOnClickListener {
      Thread(BackgroundFetcher()).start() // (1)
    }
  }

  private fun fetchGithubInfo() : String  {  // (2)
    val url = "https://api.github.com/users/tedhagos"

    val client = OkHttpClient()
    val request = Request.Builder().url(url).build()
    val response = client.newCall(request).execute()
    return response.body().string()
  }

  // INNER CLASS DEFINITIONS

  inner class BackgroundFetcher : Runnable {
    override public fun run() {  // (3)
      val github_info = fetchGithubInfo()
      println(github_info)
    }
  }
}
```

**(1)** If you can still remember this from Java, this is how to run a code in a background thread. Create a new Thread object, pass an instance of Runnable type and invoke the start method on the thread object

**(1)** This is a small function definition. It’s just a little place we can collect all our network related codes

**(3)** The run method of a Runnable type is where you should put all the codes that you want to run in the background, so, we call fetchGithubInfo right here

This code is fine if all need to do is send the output to println. You will run into problems if you try set the text attribute of a TextView while you’re inside the run method of BackgroundFetcher. While your code is running on a background thread, you cannot make changes to the UI. All UI changes needs to happen on the UI thread alone. As an exercise, try to replace println(github_info) with something like textView.text = github_info. You might encounter an exception that says

```kotlin
android.view.ViewRootImpl$CalledFromWrongThreadException:
Only the original thread that created a view hierarchy can touch its views
```

To solve the problem, we need to set the TextView while we are running on the UI thread. There are a couple of ways to do this but we’ll use the simplest. We'll invoke the runOnUiThread method of MainActivity and change the TextView in there.

Listing 4. BackgroundFetcher calling the UI Thread.  
```kotlin
inner class BackgroundFetcher : Runnable {
  override public fun run() {
    val github_info = fetchGithubInfo()
    runOnUiThread(Runnable { // (1)
      val jsonrdr = JSONObject(github_info)  (2)
      textView.text = jsonrdr.getString("login")
    })
  }
}
```
**(1)** runOnUiThread is a method of the Activity class. We are in an inner class which means the scope of the enclosing class (MainActivity) is accessible to us. Some things to note about this method are
It takes an instance of a Runnable type, that’s why I created an anonymous class here, and because of Kotlin’s magic, we don’t have to spell out the whole class structure. Notice that it doesn’t have the run method inside it?
This method will first check if the current thread is already the UI thread. If it is, it will will execute our statement already, but the current thread is not the UI thread; we are running inside a background thread. In this case, our statement will will be put into a message queue where the looper of the UI Thread can pick it up later for execution

**(2)** The github_info variable contains the whole response from GitHub, we only want to display the "login" field. So, that’s what we did here with the help of the built-in JSON reader


 