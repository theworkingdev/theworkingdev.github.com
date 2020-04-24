---
title: How to handle clicks and long-clicks in Kotlin
layout: post
categories: android, kotlin
permalink: 
date:   2018-08-01 
---

An onClickListener for a Button View object can be coded like this

```kotlin
button.setOnClickListener(object: View.OnClickListener {
  override fun onClick(v: View?) {
  println("Hello click")
  }
})
```

It can be made more concise with Kotlin lambdas, like this

```kotlin
button.setOnClickListener {
  Toast.makeText(this, "Hello World",   Toast.LENGTH_LONG).show()
  println("Hello")
}
```

Longclicks can be handled like this

```kotlin
button.setOnLongClickListener(object: View.OnLongClickListener {
  override fun onLongClick(v: View?): Boolean {
    Snackbar.make(root_layout, "long click", Snackbar.LENGTH_LONG).show()
    return true  }
})
```

The onLongClick callback function returns either true or false. Return true if you don’t want other listeners, such as onClick, to further consume the event. If you return false, both your codes in onLongClick and onClick will run.

We can make it shorter with lambdas, like this

```kotlin
button.setOnLongClickListener {
   Snackbar.make(root_layout, "Long click", Snackbar.LENGTH_INDEFINITE).show()
   true
}
```

The button referred to in examples above is a View ID that’s defined in **activity_main.xml** (see Listing 2). You can refer to it using just the View ID if you imported the Kotlin Android Extension (KAE). Listing 1 shows the full program code for MainActivity.Kt 

**Listing 1. Complete Listing for MainActivity.Kt**

```kotlin
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import android.support.design.widget.Snackbar
import android.test.ViewAsserts
import android.view.View
import android.widget.Toast
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    button.setOnClickListener {
      Toast.makeText(this, "Hello World", Toast.LENGTH_LONG).show()
      println("Hello")
    }

    button.setOnLongClickListener {
      Snackbar.make(root_layout, "Long click", Snackbar.LENGTH_INDEFINITE).show()
       true
     }
  }
}
```

Listing 6 show a snippet of the layout file. Significant portions of it have been omitted (for presentations purposes). You should not use this code directly in your app. You should always let the IDE generate the XML files for the layout.

One thing of note in Listing 2 is that all the View elements have an android:id attribute. The KAE synthesizes these IDs and makes them available to MainActivity as extension properties. That’s the reason why you can refer to elements like the button, using just its ID — you won’t have to use findViewById() anymore. 

**Listing 2. snippet of app/res/layout/activity_main.xml**

```xml
<TextView
  android:id="@+id/textView"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:text="Hello World!"
  app:layout_constraintVertical_bias="0.353" />

  <Button
    android:id="@+id/button"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    app:layout_constraintTop_toBottomOf="@+id/textView" />
```


{% include book_las3_kotlin.html %}