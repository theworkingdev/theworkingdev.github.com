---
title: Android AsyncTask
layout: post
categories: android, kotlin
permalink: /2018/08/android-asynctask.html 
date:   2018-08-09 
comments: true
---

In previous posts ([Android Jank]({% post_url 2018-07-29-android-jank_31 %}) and [Android Threads, Handlers and Messages]({% post_url 2018-07-28-android-threads-handlers-and-messages %})) we explored ways on how to run code in the background. Why would we want to do that? So that UI Thread is freed up to do UI stuff like creating and updating views. Coz if we burden the UI Thread with other stuff that doesn’t concern the UI, like doing I/O work or doing complex calculations, the Android runtime will reduce the framerate of our app which will cause it behave sluggishly — in short, jank.

In this post, we will look at another way to run code in the background. AsyncTask is part of the Android Platform (like Handlers and Messages). To use an AsyncTask, you generally need to do the following;
Extend the AsyncTask class.
Override AsyncTask’s doInBackground method so you can accomplish the background work

Override a couple more of AsyncTask’s lifecycle methods so can update the UI and report on the overall status of the background task
Create an instance of AsyncTask subclass and call execute on it, that’s how you kickstart the background operation



One of the reasons why AsyncTask is less preferred than basic Threads is that it uses generics. The AsyncTask class is a parameterized class. You have to specify three types before you can use it.

_Listing 1. The AsyncTask class._
```kotlin
AsyncTask<Void, String, Boolean> {                              // (1)

  override fun doInBackground(vararg p0: Void?) : Boolean {     // (2)
    // statement
    publishProgress("status of anything")                       // (3)
  }
  override fun onProgressUpdate(vararg values: String?) {
    // update the UI                                            // (4)
  }
  override fun onPostExecute(result: Boolean?) {
    println(result)                                             // (5)
  }
}
```

**(1)** he AsyncTask is a parameterized class. You have to specify three types before you can use it. The three types, in the order they appear, are the following;

  * **Params**. This is the information you need to pass to the AsyncTask so it can do the background task. It could be anything like a list of URLs, View object(s), String(s), and to make it a bit more challenging for us, it’s a vararg parameter. Typically, developers use this parameter to pass the View elements so that the AsyncTask can refer to the View objects of the Activity. But in our example, I will make the AsyncTask an inner class, so it can refer to any View element in MainActivity — this is reason why I used Void as the first type parameter
  * **Progress**. The type of information that you want the background thread to pass to the UI thread so you can tell the user what’s going on
  * **Result**. The kind data you want to indicate the result of the background operation, most of the time this is either true or false. If the operation  succeeds, then it’s true, otherwise it’s _false

**(2)** This is the only mandatory function to override. As the name suggests, this is where you will do things in the background. Whenever you need to read/write to a file or a network I/O, you’d want to do it here. You will notice that this function takes in a vararg Void parameter, it corresponds to the first type parameter we defined for our class. Had you made the first type parameter as String, then doInBackground would have taken a String. Notice also that this method returns a Boolean, that’s because we passed a Boolean as the third parameter type.

**(3)** Periodically, you may want to inform the user of what’s going on in your app, especially if it’s a lengthy operation. The publishProgress method lets you do that. While you are inside doInBackground, you cannot make any changes to the UI. UI changes needs to happen on the UI Thread. When you call publishProgress, the Android runtime will call onProgressUpdate, that’s where you can make UI changes. When you pass an argument to this method, the onProgressUpdate receives that argument

**(4)** When you’re inside this method, all the statements will be executed on the UI Thread. This is where you make changes to your View objects. The method takes a String parameter because we passed String as the second type parameter of the AsyncTask class, it corresponds to that. This method will be called after we’ve invoked publishProgress from the doInBackground method, whatever data you pass to publishProgress will be received by onProgressUpdate

**(5)** When doInBackground finishes, the runtime will call this method. The result parameter was returned by doInBackground

_Listing 2. Full and annotated code listing for MainActivity._  
```kotlin
import android.os.AsyncTask
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import android.view.View
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    button.setOnClickListener {
      Worker().execute()                                           // (1)
    }
  }

  inner class Worker : AsyncTask<Void, String, Boolean>() {        // (2)

    override fun doInBackground(vararg p0: Void?) : Boolean {
      for (i in 1..20) {
        publishProgress(i.toString())                              // (3)
        Thread.sleep(2000)                                         // (4)
      }
      return true
    }

    override fun onProgressUpdate(vararg values: String?) {
      textView.text = values[0]                                    // (5)
    }

    override fun onPostExecute(result: Boolean?) {
      println(result)
    }
  }
}
```

**(1)** Create an instance of Worker, then execute it

**(2)** Define an AsyncTask as an inner class, so we can refer to the View objects of the enclosing MainActivity. The type parameters are explained below
  * **Void**. I don’t really need to pass anything to the AsyncTask, so, Void
  * **String**. the onProgressUpdate method will update the the TextView. Since we’ll use this second Type to update the value TextView, String seems like a good choice
  * **Boolean**. When we’re done with doInBackground, we want to set a status to indicate failure or success, Boolean seems to be good choice for that

**(3)** Let’s tell the user what the current value of i is. The onProgressUpdate takes a String argument, that’s why we’re converting i to an Int

**(4)** This simulates a length operation

**(5)** Now that were in the UI Thread, we can safely set the text attribute of TextView to the current value of i. We only passed one parameter from publishProgress, so, if we want to get that, it’s the 0th element of the values parameter


{% include book_las3_kotlin.html %}