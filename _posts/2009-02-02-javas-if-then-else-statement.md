---
title: Java's if then else statement
permalink: /2009/02/javas-if-then-else-statement.html
categories: java
comments: true
layout: post 
date: 2009-02-02
---

The **if-then** statement is one Java's branching construct. You use a branching construct when you want to execute some of your statements only when some conditions are true.

The snippet below shows the basic use of **ifs** first form;

```java
if (condition) {
  // do this if condition is true
}
```

This construct let's you side-step some of your codes. You can choose which ones of them to do depending on the conditions you setup. The condition inside the **if** parentheses must be an boolean expression; it needs to evaluate into a boolean type. In languages like C, integers are used in if statements; in C, -1 is true and 0 is false. That's not the case in Java, so you don't have to worry about codes like this

```java
if (a = 1) {
  System.out.println(a);
}
```

The coder probably wanted to see if the value of the variable is equal to 1, but made the mistake of using the assignment operator. If this was in C, it would have been allowed because an assignment operation will return -1 if the operation succeeds and 0 if it fails. Since C allows integers to be used in statements like if, this would have compiled.  This won't happen to you in Java because the compiler would have caught it. It won't let us disgrace ourselves like this.

The second form is **if-then-else**, this provides a second path for your code. The basic use is as as follows;

```java
if (condition) {
  // do this if condition is true
}
else {
  // do this if condition is false
}
```

You can also put an **if** inside an another **if**, like this

```java
if (condition1) {
  // statements
  ...
  if (condition2) {
     // statements
     ...
  }
}
```



This kind of construct lets you build complex program logic. There is no limit for how deep you can nest the structure, but be careful with nested conditions, they can be tricky to follow and debug.



Sometimes you may need to provide more than just two pathways, you can use the third form for that.



```java
if(condition1) {
  // statement
}
else if(condition2) {
  // statement
}
else {
  // statement
}
```



The sample code below uses the third form to display the friendly name for the day, given a specific date.



```java
import java.util.*;

class PrintTheDay {
  public static void main(String[] args) {
    
    Calendar c = Calendar.getInstance();
    Date d = new Date();
    c.setTime(d);
    int dayOfWeek = c.get(Calendar.DAY_OF_WEEK);

    // Next, print out the friendly name for the
    // current day of week

    if (dayOfWeek == 1) {
      System.out.println("Sunday");
    }
    else if (dayOfWeek == 2) {
      System.out.println("Monday");
    }
    else if (dayOfWeek == 3) {
      System.out.println("Tuesday");
    }
    else if (dayOfWeek == 4) {
      System.out.println("Wednesday");
    }
    else if (dayOfWeek == 5) {
      System.out.println("Thursday");
    }
    else if (dayOfWeek == 6) {
      System.out.println("Friday");
    }
    else if (dayOfWeek == 7) {
      System.out.println("Saturday");
    }
  }
}
```



