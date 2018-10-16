---
title: Android onCreateOptionsMenu
layout: post
categories: android, kotlin
permalink: /2018/06/android-oncreateoptionsmenu.html 
date:   2018-06-15 
---

Menus can be created either [by using an XML resource file](https://www.workingdev.net/2018/06/using-simple-menu-in-actionbar.html) or by dynamically adding menu items through program code. This post is the latter.  

To add a menu to an Activity, you need to override the **onCreateOptionsMenu**() function.

**listing 1. onCreateOptionsMenu, excerpted from MainActivity**
```kotlin
  override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    Log.info("onCreateOptionsMenu")
    menu?.add("File")   //this adds the menu item dynamically
    menu?.add("Exit")   
    return super.onCreateOptionsMenu(menu)
  }
```

Note the use of safe-call operator(?.) in **menu**?.add, we need to use safe-call becasue the Menu object is declared as a nullable type in the function parameter (menu: Menu?)

Override the **onOptionsItemSelected()** function to handle the events when a menu item is clicked.

**listing 2. onOptionsItemSelected, excerpted from MainActivity**\
```kotlin
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
```

We returned true from **onOptionsItemSelected()** becasue we don't want any other listener to handle the event, returning true means we've already consumed the event, there is no more need for further handling.

**listing 3. full program for MainActivity.Kt**
```kotlin
package net.workingdev.oncreateoptionsmenulifecycle

import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import android.view.Menu
import android.view.MenuItem
import android.widget.PopupMenu
import kotlinx.android.synthetic.main.activity_main.*
import java.util.logging.Logger

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




 