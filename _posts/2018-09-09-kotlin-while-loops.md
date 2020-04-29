---
title: Kotlin while loops
layout: post
categories: kotlin
permalink: /2018/09/kotlin-while-loops.html
date:   2018-09-09 
image: /images/book_las3_kotlin.png
comments: true
---

Kotlin has a while loop that looks and behaves exactly like the one in Java

```kotlin
val arr = (1..100).filter { it % 2 == 0 }.toTypedArray()
var counter = 0
var sum = 0
while(counter < arr.size) {
  sum += arr[counter++]
}
println("sum is $sum")
```

The code above, with the exception of the first line, should look very familiar to Java folks. That's exactly how Java would do a while loop. So, we'll leave it at that.

This is what's going on in the first line;

* `(0..100)` - This creates a series of Ints from 1 to 100, using the range function in operator form
* `filter {it % 2 == 0}` - Filter is an extension function (we'll come up on that in a little while). It does exactly what you think it does, it filters data. It works on objects that are a subtype Iterable. What's inside is a lambda expression that's used to evaluate each item of the list. If the item satisfies the condition (is it an even number), then it will be included in the list
* `toTypedArray()` - The filter function returns a list. For this example, I didn't want to work with Lists, I wanted to work with arrays instead, so, I converted it with this function
 

 {% include book_las3_kotlin.html %}