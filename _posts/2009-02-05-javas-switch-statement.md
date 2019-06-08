---
title: Java's switch statement
layout: post
categories: java
comments: true
permalink: /2009/02/javas-switch-statement.html
date:   2009-02-12 21:14:28 +0800
---

The switch statement has a slightly different structure from the if-then statement which makes it, in some cases, more readable than an if-then construct that needs to handle multiple pathways. The basic form of the switch statement is as follows

```java
switch(expr) {
  case value:
     // statement;
     break;
  case value:
     // statement;
     break;
   default:
     // statement;
}
```

Where **expr** is either of type byte, short, char and int.   

```java
import java.util.Calendar;
import java.util.Date;

class Switch  {

  public static void main(String[] args) {
  
    Calendar c = Calendar.getInstance();
    Date d = new Date();
    c.setTime(d);
    int dayOfWeek = c.get(Calendar.DAY_OF_WEEK);
  
  String day = "";
    
    switch(dayOfWeek) {
      case 1:
        day = "Sunday";
        break; 
      case 2:
        day = "Monday";
        break;         
      case 3:
        day = "Tuesday";
        break; 
      case 4:
        day = "Wednesday";
        break; 
      case 5:
        day = "Thursday";
        break; 
      case 6:
        day = "Friday";
        break; 
      case 7:
        day = "Saturday";
        break;
      default:
        day = "Dunno";
    }
    System.out.printf("Today is %s", day);
 }
}
```

Starting with JDK 1.7,  you can also use Strings in switch statement. Like this.

```java
public String getTypeOfDay(String dayOfWeekArg) {

  String typeOfDay;
  switch (dayOfWeekArg) {
    case "Monday":
        typeOfDay = "Start of work week";
        break;
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
        typeOfDay = "Midweek";
        break;
    case "Friday":
        typeOfDay = "End of work week";
        break;
    case "Saturday":
    case "Sunday":
    typeOfDay = "Weekend";
        break;
    default:
        typeOfDay = "Dunno"
  }
  return typeOfDay;
}
```

The switch statement compares the String expression with each of the cases as if it was using String.equals method. It's best to remember that the comparisson is case-sensitive.



