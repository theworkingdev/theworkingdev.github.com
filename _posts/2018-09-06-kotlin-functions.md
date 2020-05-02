---
title: An introduction to Kotlin functions
layout: post
categories: kotlin
permalink: /2018/09/kotlin-functions.html
date:   2018-09-06
description: Kotlin functions are much richer than Java's. You have the flexibility to define default values for parameters and even name the parameters at the call site. This why you don't need to do much overloading in Kotlin
image: /images/book_las3_kotlin.png
comments: true
---

Kotlin functions are defined using `fun` keyword, followed by the name of the function, a pair of parentheses, optional parameters, the return type of function and then the body of the function, which is pair of curly braces. It's pretty much like a Java function, except that in Java,  we have no `fun` (pun intended) and the type of the function  is written to the left of the function (in Java), while in Kotlin, the return type of the function is to the right of function name. 

**Contents**
* TOC
{:toc}


## General usage

When a function declares a return type, the type and the function's name must be separated by a colon.  Like this function that takes in a Float argument and returns a Float value


```kotlin
fun inchesToCm(inches:Float) : Float {
  return inches * 2.54
} 

fun main(args:Array<String) {
  println(inchesToCm(10))
}
```


If a function has a single statement in it's body (like the previous code sample), you can write it as an expression. Like this

```kotlin
fun inchesToCm(inches:Float) = inches * 2.54
```

In a function expression, you don't have to write the return type because Kotlin can infer it from the RHS (right hand side of the assignment operator).

When a function (that isn't written as an expression) doesn't specify it's return type, the compiler will assume that it returns the Unit type. Think of Unit as the equivalent of Java's void, like this


```kotlin
fun talk() {       // (1)
  println("Hello") // (2)
}
```

![](/images/1.png) Our function doesn't have a declared return type. Hence, it's automatically Unit

![](/images/2.png) The body of a function whose return type is Unit cannot have any return statement in it; just like void methods in Java

Functions can be written in 3 places; as a top-level (which you've seen already) inside classes (which you've also seen) and inside another function.

```kotlin
fun foo() {                 // (1)
  fun baz() : String{       // (2)
    return "Bar"
  }
  println("Foo ${baz()}") 	// (3)
}

fun main(args:Array<String>) {
  foo()
  baz()	// (4)
  println("${Person(lastname="Doe", firstname = "John").fullname()}")
}

class Person(val firstname:String, val lastname:String) {
  fun fullname() = "$firstname $lastname"  // (5)
```


![](/images/1.png)  `foo()` is a top level function. It isn't nested in a class or another function

![](/images/2.png)  `bazz()` is nested inside function `foo()`. Which means, baz is scoped with foo; which means, it can only be called within the body of `foo()`

![](/images/3.png)  This call is ok, `baz()` is callable within `foo()`

![](/images/4.png)  Not OK. This call will fail,  `baz()` is not callable from `main()` , you can only call `baz()` from within `foo()`

![](/images/5.png)  Like Java, you can put functions inside a class

## Default function arguments

You can assign default values to function parameters, like this


```kotlin
fun connectToDb(hostname: String = "localhost",
                username: String = "mysql",
                password:String = "secret") {
}
```

And because there are default parameters, you can call this function without passing any arguments. Like this

    
```kotlin
connectToDb()
```

Or you can pass only two arguments, like this


```kotlin
connectToDb("mycomputer", "ted")
```

"mycomputer" is the hostname, "ted" is the username. The two arguments that we passed corresponds to the first two parameters that were defined in the function.

## Named params

In addition to default function arguments, we can also call the function by naming the arguments at the callsite. 

```kotlin
fun main(args:Array<String>) {
  connectToDb(hostname="neptune",
              username="saturn")
}
```

In the example above, we only passed two arguments, but since our function has default parameters, that's okay.
 

 {% include book_las3_kotlin.html %}