---
title: Reified Generics in Kotlin
layout: post
categories: android, kotlin
permalink: /2018/09/reified-generics-in-kotlin.html
date:   2018-09-29 
description: learn how to deal with type erasures during runtime. Kotlin has ways to make the runtime remember the type parameters for generics
image: /images/reified-generics.jpg 
comments: true
---

Let’s deal with the meaning of *reify* first. It means to make something real, and the reason we’re using reify and generics on the same statement is because of Java’s type erasure.

Type erasure means exactly what you think it means. Java, and Kotlin as well, erases generic type information at runtime. There are good reasons for this, but unfortunately, we’re not going to discuss those reasons why the language design is like that — but we will discuss its effects. Because of *type erasure*, you can’t perform any reflection activity and you can’t do any runtime check on a type. So, the following code won't work.

```kotlin
fun checkInfo(items:List<Any>) { 
  if(items is List<String>) {   // (1)
    println("item is a list of Strings")
  }
}
```

**(1)** This line won't compile, the error is "Cannot check for instance of erased type"

The `is` keyword doesn’t work on generic types at runtime, the smart cast breaks because of type erasure. If you have some confidence about what the runtime type of the List will be, you can make a speculative decision and cast it using the `as` keyword, like this

```kotlin
val i = item as List<String>
```

The compiler will let you through, but this is a dangerous thing to do. Let’s consider one more example where we can build a stronger case as to why we need to retain type information at runtime.

Let’s say I have a List of objects, `Programmer` and `Tester` objects. I want to create a function where I can pass a type parameter and filter the list using that type parameter. I want the function to return the filtered list.

The following code shows us sample on how this might be done — it won't work of course, because of the type erasure issue, but just read through it first, we will fix it later.

```kotlin
fun main(args: Array<String>) {
  val mlist = listOf(Programmer("Ted"), Tester("Steph")) // (1)
  val mprogs = mlist.typeOf<Programmer>() // (2)

  mprogs.forEach { // (3)
    println("${it.toString()} : ${it.javaClass.simpleName}")
  }
}

fun <T> List<pass:[*]>.typeOf() : List<T> { // (4)

  val retlist = mutableListOf<T>() // (5)
  this.forEach {
    if (it is T) {  // (6)
      retlist.add(it) // (7)
    }
  }
  return retlist  // (8)
}

open class Employee(val name:String) {
  override fun toString(): String {
    return name
  }
}
class Programmer(name:String) : Employee(name) {}
class Tester(name:String) : Employee(name) {}
```

**(1)** Let’s create a list of Programmer and Tester objects

**(2)** Let’s call an extension function (of the List type) called typeOf. We’re passing Programmer as a type argument, which means, we want this function to return only a list of Programmers objects

**(3)** We’re just iterating through each item of the list. We print the name property and the Java simpleName

**(4)** Now we come to the definition of the extension function. We’re defining a type parameter <T>, we’re using T as the return type of this function. Also, we want this function to work with any kind of List, hence the syntax

**(5)** Let’s define a mutable list, we’ll use this to hold the filtered list

**(6)** This is the code that won’t compile because we don’t know what kind of List is this anymore at runtime. Kotlin, like Java, erases the type information. But let’s assume for a moment that Kotlin does retain generic type information; if that’s the case, then this code is okay

**(7)** If the condition is okay, let’s add the current item to the return value

**(8)** Finally, let’s return the filtered list

That last code sample would have worked perfectly if only `List.typeOf` can remember (at runtime) what kind of list it is. As it turns out, Kotlin can actually make generics types remember these kinds of things at runtime. We just need to use a couple of special words, like `inline` and `reified`. Let's see how to do this in code.

```kotlin
inline fun <reified T> List<pass:[*]>.typeOf() : List<T> { // (1)

  val retlist = mutableListOf<T>()
  this.forEach {
    if (it is T) {
      retlist.add(it)
    }
  }
  return retlist
}
```

**(1)** Make the function **inline** and use the **reified** keyword before the type parameter. After doing this, the function can retain type information at runtime

You can only reify inline functions. When you inline a function, the compiler will replace every call to that function with the actual its actual bytecode (not just the address of the function). It’s like copying and pasting the bytecode of the function wherever the function is called. This is how the compiler knows the exact type that you used as the type argument. Hence, the compiler can generate the bytecode for the specific class that was used as the type argument.

If we reverse-engineer the bytecodes that compiler will generate for our reified function, it might look like the following code.

```kotlin
val retlist = mutableListOf<Programmer>()
this.forEach {
  if (it is Programmer) {
    retlist.add(it)
  }
}
return retlist
```

As you can see, we’re not testing if `it is T` anymore, we’re testing if `it is Programmer`. The generated bytecode references a specific class (Programmer) not a type parameter  like `<T>`. This is the reason why reified functions are not affected by type erasure. This, of course, will increase the size of your runtime program, so use it sparingly. 

In case you want to try this by yourself, here's the full code listing

```kotlin
fun main(args: Array<String>) {
  val mlist = listOf(Programmer("Ted"), Tester("Steph"))
  val mprogs = mlist.typeOf<Programmer>()

  mprogs.forEach {
    println("${it.toString()} : ${it.javaClass.simpleName}")
  }
}

inline fun <reified T> List<*>.typeOf() : List<T> {

  val retlist = mutableListOf<T>()
  this.forEach {
    if (it is T) {
      retlist.add(it)
    }
  }
  return retlist
}

open class Employee(val name:String) {
  override fun toString(): String {
    return name
  }
}
class Programmer(name:String) : Employee(name) {}
class Tester(name:String) : Employee(name) {}
```
 
Photo credit: yenwen / E+ / Getty Images
Disclaimer: no copyright infringement intended