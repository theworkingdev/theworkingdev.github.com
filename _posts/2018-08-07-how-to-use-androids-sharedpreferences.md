---
title: Android SharedPreferences in Kotlin
layout: post
categories: 
 - android
 - kotlin
permalink: /2018/08/how-to-use-androids-sharedpreferences.html
date:   2018-08-07 
---

A preference file is the easiest and most straightforward way to store data in Android app. A preference file stores pairs of key-value items, like dictionaries or maps, and they are stored in the file system as XML files.

You can create a preference file that’s private to the Activity that created it, or you can create one that’s accessible by any Activity in your app.

A preference file that’s private to an Activity is created using `getPreferences()` and a file that’s accessible by any Activity in your app is created using `getSharedPreferences()`.

The following example demonstrates how to create and use a preference file that’s available to all Activities in your app.

```kotlin
val filename = "$packageName TESTFILE"     // (1)
val pref = getSharedPreferences(filename, Context.MODE_PRIVATE)   // (2)
val editor = pref.edit()

editor.putString("lastname", "Breslav")
editor.putString("firstname", "Andrey")
editor.apply()
```

**(1)** packageName is actually a call to getPackageName(). We’re just constructing a file name in this line

**(2)** Instead of calling getPreferences, let’s use getSharedPreferences. This function takes in two parameters. You already know the second one, and it’s easy to guess what the first parameter is for. The first parameter specifies a filename for the preferences file

Actually, getPreferences (our example in the previous section) is just a wrapper call to getSharedPreferences, the latter simply passes the name of current Activity as the first parameter to the former.

To retrieve data from the shared preferences file, use the getSharedPreferences again, specifying which file to read from, then, use the getString methods, as shown in the following code example.


```kotlin
val pref = getSharedPreferences("$packageName TESTFILE", Context.MODE_PRIVATE) // (1)

val mlastname = pref.getString("lastname", "")  // (2)
val mfirstname = pref.getString("firstname", "")
```

**(1)** get a SharedPreferences object. Specify the name of the preferences file by passing it as a the first parameter

**(2)** First parameter is the key, it’s the name of the preference to retrieve. The second parameter is a default value, in case the key doesn’t exist
 

 {% include book_las3_kotlin.html %}