---
title: Kotlin Exception Handling
layout: post
categories: kotlin
permalink: /2018/09/kotlin-exception-handling.html
date:   2018-09-08 
description: Like Java, Kotlin uses the same try-catch construct. But unlike Java, Kotlin treat all exceptions as unchecked. Hence, try-catch is optional
image: /images/book_las3_kotlin.jpg
comments: true
---

Kotlin's approach to exception is similar to Java. Somewhat. It uses the _try-catch-finally_, just like in Java. So, your knowledge about how _try-catch_ works commutes nicely to Kotlin. The code below should be very familiar. It shows a typical code on how to open a file

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

So, what's different? Well, in Kotlin, everything is an _unchecked exception_. Which means, the try-catch block is optional. It's up to the programmer if you want to use it. So, the code above, can be written like this (in Kotlin).

```kotlin
import java.io.FileReader  

fun main(args: Array<String>) {
  var fileReader = FileReader("README.txt")  
  var content = fileReader.read()  
  println(content)
}
```

So how do you know if you need to use try-catch. You might not like the answers now, but in the long run, these are good advice to follow ;

1. You have to know the API's you're using. I know that this is not great news for beginners, but look at it this way, you won't be a beginner for long. And you really should know the API's you're using. TL;DR won't serve you well in this area
2. You have to get into the habit of unit-testing your code. This is a good habit to develop anyway


{% include book_las3_kotlin.html %}