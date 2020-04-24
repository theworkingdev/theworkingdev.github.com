---
title: Kotlin's function can take variable arguments
layout: post
categories: kotlin
permalink: /2018/10/kotlin-variable-arguments-in-functions.html
date:   2018-09-29 
description: You can pass variable number of arguments in a Kotlin function. See how it's done
comments: true
image: /images/book_las3_kotlin.png
---

Functions in Kotlin, like in Java, can also accept an arbitrary number of arguments.  The syntax is a bit different from Java, instead of using three dots after the type `...` , we use the `vararg` keyword instead. 

```kotlin
fun<T> manyParams(vararg va : T) {  // (1)
  for (i in va) { // (2) 
    println(i)
  }
}

fun main(args: Array<String>) {
  manyParams(1,2,3,4,5)  // (3)
  manyParams("From", "Gallifrey", "to", "Trenzalore")  // (4) 
  manyParams(*args) // (5) 
  manyParams(*"Hello there".split(" ").toTypedArray()) // (6)
}
```

**(1)** The vararg keyword lets us accept multiple parameter for
this function. In this example, we declared a function that has a typed
parameter; it’s generic. We didn’t have to declare it as generic in order to
work with variable arguments, we just chose to so that it can work with a
variety of types

**(2)** This is a simple looping mechanism so that we can print each item in the argument

**(3)** We can pass Ints, and we can pass as many as we want because manyParams accepts variable number of arguments

**(4)** It works with Strings as well

**(5)** Like in Java, we can pass an array to a function that accepts variable arguments. We need to use spread operator * to unpack the array. It’s like passing the individual elements of the array one by one, manually

**(6)** The split() member function will return an *ArrayList*, you can convert it to an *Array* , then use the spread operator so you can pass it to a *vararg* function

{% include book_las3_kotlin.html %}