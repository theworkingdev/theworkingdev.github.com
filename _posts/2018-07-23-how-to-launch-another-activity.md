---
title: How to launch an Activity in Kotlin
layout: post
categories: android, kotlin
permalink: /2018/07/how-to-launch-another-activity.html
date:   2018-07-23 
---

Android is made up of loosely coupled components. What if you need your components to talk to each other e.g. launch another Activity? How do you think we should manage that? If you have any experience with desktop programming, you might think that you can do it with a code like this

```kotlin
class MainActivity : Activity {
  fun whenClicked(v:View) {
    SecondActivity()  // This ought to do it?
  }
}

class SecondActivity : Activity {

}
```

Unfortunately, the preceeding code might look sensible and a really straight-forward way to do it, but it’s wrong and it won’t work. An Activity in Android isn’t just a simple object. You cannot launch it by simply creating an instance. Components in Android needs to be Activated. This is where Intents come in.

There are two kinds of Intents, an explicit and implicit one. To Launch another Activity within your application, explicit Intents are typically used. The following code listing shows an annotated snippet on how to launch another Activity within your app using an explicit Intent.

```kotlin
button.setOnClickListener {
  val intent = Intent(this@MainActivity, SecondActivity::class.java) // (1) (2)
  startActivity(intent)  // (3)
}
```

**(1)** this@MainActivity The first parameter of the Intent constructor is a Context object. We passed this@MainActivity because the Activity class is a subclass of Context, so we can use that. Alternatively, we also could have used getApplicationContext(), an Application context would have been okay just as well

**(2)** SecondActivity::class.java. The second parameter is a Class object. It’s the class of the Component to which we want to deliver the message. This a reflection syntax. As you may already know, reflection allows us inspect the structure of our programs during runtime. SecondActivity::class would refer to the runtime reference of SecondActivity if it were a Kotlin class (KClass), but it’s not. SecondActivity is a Java class (Android libraries are still in Java) hence we refer to it as SecondActivity::class.java

**(3)** We launch the Activity by calling startActivity() and passing the intent object to it
 
{% include book_las3_kotlin.html %}