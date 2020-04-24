---
title: Kotlin when, switch statement on steroids
layout: post
categories: kotlin
permalink: /2018/09/kotlins-when-is-switch-on-steroids.html 
date:   2018-09-25 
description: Kotlin doesn't have a switch statement, like the one in Java. But, it has the when construct. You can use it either as a statement or expression. It's got superpowers
image: /images/book_las3_kotlin.jpg
---

Kotlin doesn’t have a _switch_ statement, but it has the _when_ construct. It looks a lot like the _switch_ but it packs a lot more punch. In its simplest form, it can be implemented like this

```kotlin
val d = Date()
 val c = Calendar.getInstance()
 val day = c.get(Calendar.DAY_OF_WEEK)

 when (day) {
   1 -> println("Sunday")
   2 -> println("Monday")
   3 -> println("Tuesday")
   4 -> println("Wednesday")
 }
```

The idea is to match the argument (the variable `day`) against the branches `1`, `2`, `3` or `4` . The test is carried out from top to bottom (1, then 2, then 3 then 4) and when a match is made the statement (or block) to right of the thin arrow `->` is executed. Unlike in _switch_ statements, when a match is found, it doesn’t flow through or cascade to the next branch, so, you don’t need to put a _break_ statement.

The _when_ construct can also be used as an expression, and when it’s used as such, each branch becomes the returned value of the expression. See the code example below.

```kotlin
val d = Date()
 val c = Calendar.getInstance()
 val day = c.get(Calendar.DAY_OF_WEEK)

var dayOfweek = when (day) {
   1 -> "Sunday"
   2 -> "Monday"
   3 -> "Tuesday"
   4 -> "Wednesday"
   else -> "Unknown"
 } 
```

Just remember to include the _else_ clause if you use it as an expression. The compiler thoroughly checks all possible pathways and it needs to be exhaustive, which is why the _else_ clause becomes a requirement.

You’re not limited to numeric literals, you can use a wide variety of types for the branches, like this code below.

```kotlin
fun main(args: Array<String>) {
    
  print("What is the answer to life? ")
  
  var response:Int? = readLine()?.toInt()  // (1)                                                 
  val message = when(response){
    42 -> "So long, and thanks for the all fish"
    43, 44, 45 -> "either 43,44 or 45" // (2)   
    in 46 .. 100 ->  "forty six to one hundred" // (3)    
    else -> "Not what I'm looking for" // (4)   
  }
  println(message)
}
```

**(1)** `readLine()` reads an input from the console. Don’t worry about the questions marks for now, we’ll get to that in the coming sections

**(2)** The branch conditions may be combined with a comma

**(3)** We can check if it’s a member of a range or a collection

**(4)** The else clause is required when when is used as an expression

You can also use _when_ without an argument, like this

```kotlin
val a = 3
val b = 2
val c = 1
val str = "str"

when {
  a > b -> println("a is greater than b")
  b > c -> println("b is greater than c")
  b == 2 -> println("b is equal to 2")
  str == "str" -> println("str is actually equal to $str")
}
```

Which effectively makes it the equivalent of an _if-elseif_ structure; it looks much cleaner though. But hold on a minute, read the sample code above again, and this time, examine it closely. Have you noticed that all the conditions in all the branches should evaluate to true? They do. However, if you run code above, it will only print the first branch, `a > b`. Like the _if-elseif_, when one branch evaluates to true, the rest of the succeeding branches are ignored. They will no longer be evaluated. Best to take note of that.

You can also use the `is` operator in a _when_ construct. If you do that, you get the benefit of smart casts as well. See the following code for an example.

```kotlin
when (arg) {
  is Int -> println("The square of $arg is ${arg * arg}")
  is String -> println("Hello" + arg)
  is IntArray -> println(arg.sum())
}

```

Another (slightly more complicated) example of using the `is` operator in a _when_ construction.

```kotlin
fun <T> fooBar(arg:T) : T {   
  var retval:T = 0 as T
  when (arg) {
    is String -> {           
      retval = "Hello world" as T   
    }
    is Number -> {
      retval = 100 as T
    }
  }
  return retval
}
```

Here's how you might use it in an Android app.

```kotlin
class MainActivity : AppCompatActivity() {
  val Log = Logger.getLogger(MainActivity::class.java.name)

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
  }

  override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    Log.info("onCreateOptionsMenu")
    menu?.add("File")
    menu?.add("Exit")
    return super.onCreateOptionsMenu(menu)
  }

  override fun onOptionsItemSelected(item: MenuItem?): Boolean {
    when (item?.toString()) {
      "File" -> {
        Log.info("LOG File menu")
      }
      "Exit" -> {
        Log.info("LOG Exit menu")
      }
    }
    return true
  }
}
```

 {% include book_las3_kotlin.html %}