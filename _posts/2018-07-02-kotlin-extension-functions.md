---
title: Kotlin Extension Functions
layout: post
categories: android, kotlin
permalink: /2018/07/kotlin-extension-functions.html
date:   2018-07-02 
description: An extension function in Kotlin allows to add behavior to an existing class, including the ones written in Java, without using inheritance. It essentially lets us define a function that can be invoked as a member of the class, but the function is implemented outside the class
---

In Java, if we needed to add functionality to a class, we could either add methods to the class itself or extend it by inheritance. An extension function in Kotlin allows to add behavior to an existing class, including the ones written in Java, without using inheritance. We can define additional behavior for an existing class and do the definition outside of that class.

To demonstrate extension functions, let's start with some simple codes and build it up as we go along.  


```kotlin
fun main(args: Array<String>) {
   val msg = "My name is Maximus Decimus Meridius"
   println(homerify(msg))
   println(chanthofy(msg))
   println(terminatorify(msg))

 }

 fun homerify(msg: String) = "msg -- woohoo!"
 fun chanthofy(msg: String) = "Chan, msg , tho"
 fun terminatorify(msg: String) = "$msg -- I'll be back"
```

The code sample above features three functions that takes a String argument, adds some Strings to it and then return them back to the caller, it’s simple.  It is usable as it is, but we can probably consolidate it a bit more by putting all the three functions in a common class, which will become our utility class, such a class might look something like this

```kotlin
fun main(args: Array<String>) {
   val msg = "My name is Maximus Decimus Meridius"
 
   val util = StringUtil()
   println(util.homerify(msg)) 
   println(util.chanthofy(msg))
   println(util.terminatorify(msg))
 }
 
/*
  The StringUtil class consolidates our 3 methods as member functions.
  This is a very common Java practice
*/
 class StringUtil {
   fun homerify(msg: String) = "msg -- woohoo!"
   fun chanthofy(msg:String) = "Chan, msg , tho"
   fun terminatorify(msg: String) = "$msg -- I'll be back"
 } 
```


The code above is usable as it is. It’s considered a good idea to consolidate methods that are somewhat related into a utility class.  Kotlin lets us go the extra mile with extension functions.  Wouldn't it be nice if we can annex our very own methods to the String class? Like this. 

```kotlin
fun String.homerify() = "$this -- woohoo!"
```

It looks deceptively simple, but this is really all it takes to write an extension function. Extension functions introduces the concept of a receiver type and a receiver object. 

![](/images/extension-functions.png)

In our example extension function, the receiver type is String, it’s the class that we’d like to add our extension function to. The receiver object is the instance of that type, which in our examples is “My name is Maximus Decimus Meridius”.  When you attach an extension function to a type, such as a String in our case, the extension function can reference the receiver object using the keyword this.  For all intents and purposes,  an extension functions appears to be just like any member function defined on the receiver type.  So, it makes sense for the extension function to be able to reference this.  

Okay, now let's add our chantofy, homerify and terminatory to the String class.

```kotlin
fun main(args: Array<String>) {
   val msg = "My name is Maximus Decimus Meridius"

   println(msg.homerify())
   println(msg.chanthofy())
   println(msg.terminatorify()) 
 }
 
 fun String.homerify() = "this -- woohoo!"
 fun String.chanthofy() = "Chan, this , tho"
 fun String.terminatorify() = "$this -- I'll be back"
```


It’s perfectly alright to still write utility functions in Kotlin, but with extension functions at our disposal, it seems more natural to use them because it increases the semantic value of our code. It feels more natural to use extension function syntax.
 