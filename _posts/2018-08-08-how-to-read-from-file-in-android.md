---
title: Using the file API to read a file in Android
layout: post
categories: 
 - android
 - kotlin
permalink: /2018/08/how-to-read-from-file-in-android.html
date:   2018-08-08 
---

The following snippet shows how to read from a file in the internal storage (standard location, not cache).

```kotlin
val filename = "ourfile.txt"
val input = openFileInput(filename) // (1)

input.use {
  var buffer = StringBuilder() // (2)
  var bytes_read = input.read() // (3)
  while(bytes_read != -1) { // (4)
    buffer.append(bytes_read.toChar())  //(5)
    bytes_read = input.read() // (6)
  }
  println(buffer.toString()) // (7)
}
```

**(1)** openFileInput returns a FileInputStream, this is the object we need so can read from a file. The only parameter it takes, is the name of the file to read

**(2)** We won’t be able to read the entire file in one fell swoop. We’ll read it by chunks. As we get some chunks, we’ll store them inside the StringBuilder object

**(3)** The read method reads a byte of data from the input stream and returns it as an integer. We need to keep reading from the stream one byte at a time until we reach the end of file (EOF) marker

**(4)** When there is no more bytes to read from the file, the EOF is marked as -1. We will use as the condition for our loop. We’ll just keep on reading until we reach EOF


**(5)** The read method returns an int; it’s the ASCII value of each letter in the file, represented as integer. We have to convert it to a Char before we can put it in the StringBuilder

**(6)** If we’re not at EOF yet, let’s read another byte

**(7)** When we run out of bytes to read, we’ll get out of the loop and print the content of StringBuilder
 

 {% include book_las3_kotlin.html %}