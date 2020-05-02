---
title: Toast message in Kotlin
layout: post
categories: kotlin, android
permalink: /2018/06/toast-message.html
date:   2018-06-14 
image: /images/book_las3_kotlin.png
---

An Android Toast is a small message displayed on the screen, similar to a tool tip or other similar popup notification. A Toast is displayed on top of the main content of an activity, and only remains visible for a short time period; a sample Toast message is shown on figure 1.

<!-- ![](/images/android-simple-toast-example.jpg) -->

<figure style="margin-right:auto; margin-left:auto; width:20rem;" >
<img src="/images/android-simple-toast-example.jpg">
<figcaption>Figure 1. Toast Message</figcaption>
</figure>


**Listing 1. Toast example**

```kotlin
import javax.swing.text.View
import android.widget.Toast
 
class MainActivity {
  fun whenClicked(v:View) {
    t = Toast.maketext(this@MainActivity,  “Hello”, Toast.LENGTH_LONG)
    t. show()
  }
} 
```

**Toast.maketext** takes three params, they are

1. Application Context, in our example this is the MainActivity’s context, we can write it as  **this@MainActivity**
2. The **String** message you’d like to show, and finally
3. How long do you want to show the message. This is an integer value, but you should use the constants of the Toast class

 You need to call **show()** on the Toast object to make it visible to the user. The Toast message disappears after some timeout.

There's an even easier way to use Toast, and that is by using the Anko library.  This library is written in Kotlin (by Jetbrains, the same guys who created Kotlin). Anko does a lot of things, you can find out more about it on the [GitHub page for Anko.](https://github.com/Kotlin/anko/wiki/Anko-Commons-%E2%80%93-Dialogs) For now, let's just use it for Toast.  

First, you need to add the Anko dependency to your project. Add it to your build.gradle file (module level), like this

```gradle
dependencies {
  ....
  implementation 'org.jetbrains.anko:anko-common:0.9'
}
```

After that, you can use Toast like this

```kotlin
toast("Hello")
toast(R.string.message)
longToast("Heeeeelllllllllllooooo")
```

{% include book_las3_kotlin.html %}