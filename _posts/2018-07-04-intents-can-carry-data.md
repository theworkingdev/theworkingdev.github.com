---
title: Android, Intents can carry data
layout: post
categories: android, kotlin
permalink: /2018/07/intents-can-carry-data.html 
date:   2018-07-04 
image: /images/book_las3_kotlin.png
---

Android Intents can do so much more than just just simpy activate or launch another Activity in your app, it can also carry data. This capability lets you pass around data between components.

Let’s say you have two Activities; MainActivity and SecondActivity, when a Button is clicked in MainActivity, you’d like to launch SecondActivity but you would also like pass some data to it. The steps to do this are as follows;

1. Create an Intent, for the purposes of our example here, it will be an explicit Intent,
2. Add data to the intent using the putExtra method
3. Launch the other Activity by calling the startActivity method, at this point, the Android runtime will launch SecondActivity
4. Within the onCreate method of SecondActivity, we can extract the data from the Intent by using the getExtra method
Intents can carry data

![](/images/intents-sequence-diagram.png)

<aside><strong>NOTE</strong> 
Most of the function calls in Android like startActivity, onCreate etc. are asynchronous — that’s why the arrows used in the sequence diagram are half-stick arrows. The sequence of calls as shown here are approximations only, they may not actually happen in that order
</aside>


Somewhere in MainActivity's event handling code, you could write something like this

```kotlin
button.setOnClickListener {
  val intent = Intent(this@MainActivity, SecondActivity::class.java) // (1)
  intent.putExtra("main_activity_data", editText.text.toString()) // (2)
  startActivity(intent) // (3)
}
```

**(1)** Create an explicit Intent. 1st argument is a Context object and 2nd argument is the Class object of SecondActivity

**(2)** Assuming there is an editText View object in your activity_main.xml, were getting the runtime value of editText and setting it as value of "main_activity_data". The putExtra method of the Intent object takes in a key-value pair. The first parameter is the key ("main_activity_data") and it’s value is the runtime value of editText

**(3)** Now, we launch the Intent

To receive the data in SecondActivity, you can do something like this

```kotlin
val myintent = getIntent() // (1)
val data = myintent.getStringExtra(“main_activity_data”) // (2)
```

**(1)** Get a reference to Intent object that’s associated with SecondActivity. We are not creating a new Intent in here. The Intent that you want is the one that’s already associated with SecondActivity, because that’s the Intent that was launched by MainActivity. That’s the Intent that carries the data

**(2)** We are using getStringExtra because we know that what we put in here was a String


 