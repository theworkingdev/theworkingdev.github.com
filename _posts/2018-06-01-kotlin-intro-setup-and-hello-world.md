---
title: Kotlin Intro, Setup and Hello World
layout: post
categories: kotlin
permalink: /2018/06/kotlin-intro-setup-and-hello-world.html
date:   2018-06-01 
---

It's the new(est) kid on the the JVM  block.

It's from JetBrains, they're the guys who built IntelliJ. They've been around for quite some time, so chances are you already know them. Kotlin has also been around for quite sometime, since 2011 in fact — it gained a lot of traction and attention probably 2016 or 2017 (I'm not sure, I'm too lazy to research).  The "hooray" moment for Kotlin was probably during Google I/O 2017, because they (Google) announced that it can now be used for Android programming. This was the same time that Android Studio 3 was about to be released, and AS3 already has an out-of-the-box support for Kotlin. You don't even have to add the plugin anymore. 


Like Java, Kotlin is;

* compiled
* statically typed, but we don't always have to write the type. Kotlin can infer types on most situations. When Java 10 comes out, it will be able to infer types too, at least for local variables 
* strongly typed
* object oriented

Unlike Java, it;

* treats functions as first class citizens and it supports functional programming. We also can write functions as top-level constructs, it doesn't have to be inside a class anymore. Kotlin refers functions inside a class as member functions, not member methods
* treats exceptions differently. All exceptions are unchecked exceptions, you may (or may not) handle exceptions, it's up to you and not the compiler. try-catch blocks are optional
* doesn't have primitive types, everything truly is, an object
* doesn't have static members
* treats everything as public by default, classes, variables and functions. If you want to restrict accessibility, you'll have to specify it
* treats expressions and statements differently, e.g. assignment is a statement in Kotlin (it is an expression in Java), the decision making constructs like _if_ and _when_ are expressions, not statements
* treats all variables as _non-Nullable_ by default. It means, when you declare a variable in Kotlin, without any decoration, modifier or annotation, the variable can never be assigned a _null_ value



# Compilation 

It's a JVM language, so, naturally, you still need the JDK. KT files are compiled using the kotlin compiler (`kotlinc`).



![compilation process](/images/kotlin-compilation-process.png)

The Kotlin compiler outputs `.class` files, just like `javac`. But, you cannot run Kotlin bytecodes the same way you would run Java bytecodes. You'll need to combine the resulting  Kotlin byte code with the Kotlin runtime and package it into a jar file. Only then will you be able to run the program.

This detail is probably important for you if you will work with Kotlin on the command line. These compilation and execution details are typically handled for you by an IDE.  I can think of three IDEs that should be usable with Kotlin; Eclipse, NetBeans and IntelliJ — the last one is a commercial IDE but it does have a community edition. IntelliJ CE is able to run Kotlin.  Anyone of these 3 IDEs should suffice. In case you choose to use IDEA, you might want to refer to the [IntelliJ minimal survival guide](http://bit.ly/intellijguide). 



# Project setup in IntelliJ

Launch IntelliJ if it isn't opened yet. On the "Welcome" screen, click "Create new Project". Next, you should see a screen like Figure 1. 



![newprojectidea](/images/new-project-intellij.png)

**Figure 1. New Project**

Choose "Kotlin/JVM", as shown in Figure 1, and try to see if IntelliJ has detected your installed JDK. As you can see, it detected my JDK 9.

Next, fill out the "Project name" information, name the project "kotlinproject" for example.  After that, the project should be created and opened in IntellIJ.

Now we can add a Kotlin file to the project. Use the context menu (right click) on the "src" folder, as shown in Figure 2. 



![new file](/images/new-kotlin-file.png)

**Figure 2. Add a Kotlin file to the project**

Choose "Kotlin/File". Name the file, and you should be good to go. 



# Hello World

The simplest program we'll create in Kotlin could look like the following;

**Listing 1. hello.Kt**

```kotlin
fun main(args: Array<String>) {
  println("Hello")
}
```

Kotlin programs don't need classes in order to run. You can get by with top level functions only.  

To run this code in IntelliJ, from the main menu bar, click **Run** →  **Run**. You will notice that there are two Run options on the menu. The first Run is greyed out. That's because we haven't defined any runtime configuration yet. Choose the second Run option. You should see a popup dialog, as shown in Figure 3



![new file](/images/kotlin-run-context.png)

**Figure 3. Running Hello.Kt**



 