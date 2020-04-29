---
title: Kotlin if-then expressions
layout: post
categories: kotlin
permalink: /2018/09/kotlin-if-then.html
date:   2018-09-22 
description: Kotlin's if-then construct is almost the same as Java's, but in Kotlin, if-then is an expression, not a statement
image: /images/book_las3_kotlin.png
comments: true
---


The _if_ construct in Kotlin works almost the same as in Java.

```kotlin
val theQuestion = "Doctor who?"
val answer = "Theta Sigma"
val correctAnswer = ""

if (answer == correctAnswer) {
 println("You are correct")
}
```

Another example, this time, let's use the `else if` and `else` clause.

```kotlin
val d = Date()
val c = Calendar.getInstance()
val day = c.get(Calendar.DAY_OF_WEEK)

if (day == 1) {
 println("Today is Sunday")
}
else if (day == 2) {
 println("Today is Monday")
}
else if ( day == 3) {
 println("Today is Tuesday")
}
else {
  println("Unknown")
}
```

So far, it looks a lot like how you would you do it in Java. Doesn't it? What's different in Kotlin is, the `if` construct isn't a statement, it's an expression. Which means, you can assign the result of the `if` expression to a variable. Like this

```kotlin
val theQuestion = "Doctor who"
val answer = "Theta Sigma"
val correctAnswer = ""

var message = if (answer == correctAnswer) {
 "You are correct"
}
else{
 "Try again"
}
```

If you have only one statement (each) on the if and else block, you can even shorten the code, like this

```kotlin
var message = if (answer == correctAnswer) "You are correct" else "Try again"
```

{% include book_las3_kotlin.html %}