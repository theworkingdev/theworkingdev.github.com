---
title: Creating and looping through Kotlin Map
layout: post
categories: android, kotlin
permalink: /2018/08/how-to-create-and-loop-through-kotlin_3.html
date:   2018-08-01 
image: /images/book_las3_kotlin.png
---

It's very easy to work with Maps in Kotlin, you can see for yourself in the following examples.

```kotlin
val months = hashMapOf("January" to 1) // (1)
months["February"] = 2                 // (2)
months["March"] = 3
```

**(1)** You can simply use the `hashMapOf` constructor function to declare and initialize the map. There's no need to provide the type parameter (you still can if you want to, but why bother), the type is inferred from the constructor arguments
**(2)** Adds a new key and value to the map

Another way to create a Map is shown below

```kotlin
val snapshot: MutableMap<String, Int> = months
```

The variable `snapshot` is declared to be a **MutableMap** (pretty much like what we did with `hashMapOf`. And then, we assigned the months Map object to the snapshot variable. Were not creating a new Map object here, snapshot is essentially months also, they are both pointing to the same Map object.

You can get to the items in the Map this way

```kotlin
println(months["January"]) // prints 1
println(snapshot["March"]) // prints 3
```

You can traverse the Map this way

```kotlin
months.forEach { println(it) }
```

Remember that it is the implicit parameter in a lambda. If you want to explicitly work with keys and values of the Map, you can do it like this

```kotlin
months.forEach { k, v -> println("$k | $v") }
```

Where `k` is the key and `v` is the value.


{% include book_las3_kotlin.html %}