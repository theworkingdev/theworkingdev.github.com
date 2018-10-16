---
title: Using the File API to write a file in Android
layout: post
categories: android, kotlin
permalink: /2018/08/how-to-write-to-file-in-android.html
date:   2018-08-19 
---

The following snippet shows how to write a file in the internal storage (standard location, not cache).

```kotlin
val filename = "ourfile.txt"

val out = openFileOutput(filename, Context.MODE_PRIVATE) // (1)
out.use { // (2)
  out.write(txtinput.text.toString().toByteArray()) // (3)
}
```
**(1)** openFileOutput returns a FileOutputStream object. The first parameter of the call is name of the file we want to create. The second parameter is a Context mode. You’re already know this from the previous chapter. We’re using MODE_PRIVATE because we want the file to be private to our app

**(2)** the use extension means I don’t have to close file explicitly or manually. As soon as we’re done using it, the Android runtime will close it for us. This is pretty handy considering that a lot of developers forget to close the files. Leaving a file handle open until the app terminates causes memory-leak. The use extension is Kotlin’s equivalent of Java’s try-with-resources

**(3)** The write method expects ByteArray. So, we need to convert the Editable (data type of EditText) to a String, then convert it to a ByteArray
 