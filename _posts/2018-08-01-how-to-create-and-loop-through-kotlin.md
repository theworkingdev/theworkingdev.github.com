---
title: Creating and Looping through Kotlin arrays
layout: post
categories: android, kotlin
permalink: /2018/08/how-to-create-and-loop-through-kotlin.html
date:   2018-08-01 
---


You can create an Array like using the **emptyArray** function, like this

```kotlin
var arr = emptyArray<String>()
```

That creates an empty Array of Strings. The Array is paraemeterized type, so you need to pass the type of data  you want for the Array. The above example, obviously, creates an empty array of String. 

```kotlin
var arrInt = emptyArray<Int>()     // array of Ints
var arrFloat = emptyArray<Float>() // array of Floats
```

Empty arrays aren't of much use, you need a way to add items to it. Here's how to do it.

```kotlin
var fruits = emptyArray<String>()
fruits += "Apple"
fruits += "Banana"
fruits += "Orange"
```

See how easy that was? It's as simple as concatenating Strings.  But don't be fooled by the easy syntax, Kotlin Arrays have fixed sizes, like their Java cousins. That means it's still expensive to do Array operations, like what we just did. In order to add an element to an Array, the runtime needs to;

1. Create a new Array that is larger than the old array
2. Copy the contents of the old array to the new one
3. Add the new item to the new Array

The syntactic sugar of Kotlin hides all these ugliness from us, but it's best to keep this in mind when working with Arrays. If you're not dealing with too much data, Arrays can be very handy.

Another way to create Arrays is to use **arrayOfNulls** function. Here's how it looks like

```kotlin
var fruits = arrayOfNulls<String>(3) 
fruits.set(0, "Apple")
fruits.set(1, "Banana")
fruits.set(2, "Orange")
```

The Int on the Array constructor (3) indicates that we want to create an Array with 3 elements, and then fill them all up with _nulls_ 

The third way to create Arrays is by using the **arrayOf** constructor function. This is probably the simplest and easiest way to create Arrays. Here's how it looks like.

```kotlin
var fruits = arrayOf("Apple","Banana","Orange")
```

This is probably the most natural way to create an Array, so, just use it whenever you can. You don't have to say what type is it, it can be inferred from the constructor arguments; plus, you can declare and define the array in one line.

Looping through arrays can be simply managed by the following code

```kotlin
for (i in fruits) {
    println(i)
}
```

Or you can use the forEach technique, it looks like something like this

```kotlin
fruits.forEach { println(i) }
```









 