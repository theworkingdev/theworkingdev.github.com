---
title: How to open a file in Kotlin
layout: post
categories: 
 - kotlin
comments: true
date:   2019-6-8  
image: /images/book_las3_kotlin.png
---

Just like in Java, you use the `java.io` classes, like this;

```kotlin
import java.io.FileNotFoundException
import java.io.FileReader
import java.io.IOException

fun main(args: Array<String>) {
  var fileReader: FileReader
    try {
    fileReader = FileReader("README.txt")
    var content = fileReader.read()
    println(content)
 }
  catch (ffe: FileNotFoundException) {
    println(ffe.message)
  }
  catch(ioe: IOException) {
    println(ioe.message)
 }
}
```

It can actually be much simpler than the above code; because in Kotlin, you don't have to use _try-catch_ if you don't want to. Exception handling is optional. So, you write the code like this;

```kotlin
import java.io.FileReader  

fun main(args: Array<String>) {
  var fileReader = FileReader("README.txt")  
  var content = fileReader.read()  
  println(content)
}
```

{% include book_las3_kotlin.html %}