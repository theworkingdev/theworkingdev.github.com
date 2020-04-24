---
title: Menu on Android ToolBar, using XML
layout: post
categories: android, kotlin
permalink: /2018/06/using-simple-menu-in-actionbar.html
date:   2018-06-15 
---

You can create a menu for an app in 3 steps. First is to create a menu resource file which will contain all the menu items, Next steps is to inflate the menu file and attach it to a **Menu** object and lastly, handle the events for each menu item.

Create a **menu** folder in the under the **app**/res folder of the project. On the Project Window, use the context menu of Android Studio; right-click on the res folder then **New** > **Android Resource Directory**

Next, right click the **app/res/menu** folder, choose New > Menu Resource File, give the new menu a name, like "main_menu". This will create the file "app/res/menu/main_menu.xml".

Put some items in the menu file, follow the codes in listing 1.

**listing 1. /app/res/menu/main_menu.xml** 

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

  <item android:id="@+id/menuFile"
    android:title="@string/menuFile"
    />
  <item android:id="@+id/menuEdit"
    android:title="@string/menuEdit"
    />
  <item android:id="@+id/menuHelp"
    android:title="@string/menuHelp"
    />
  <item android:id="@+id/menuExit"
    android:title="@string/menuExit"
    />
</menu>
```

You will also need to create the corresponding XML entries for @string/menuFile, @string/menuEdit, @string/menuHelp and @string/menuExit in the /app/res/value/strings.xml file. You can use Andr0id Studio's Quick Fix so you don't have to do it manually. To use Quick Fix, click on the text @string/menuFile, then press **OPTION** + **ENTER** if you're on macOS; it's **ALT** + **ENTER** for Windows and Linux.

To inlate and attach the menu item to the ActionBar, you need to override the  **onCreateOptionsMenu()** in MainActivity.  This project targets API level 23 (Marshmallow) so the onCreateOptionsMenu function will be called after the **onCreate** callback. If the project is API level 10 or lower, the onCreateOptionsMenu will only be called when the user clicks the hardware menu button.

**listing 2. onCreateOptionsMenu**
```kotlin
override fun onCreateOptionsMenu(menu: Menu?): Boolean {
  menuInflater.inflate(R.menu.main_menu, menu)
  return super.onCreateOptionsMenu(menu)
}
```

We can intercept the menu item events by overriding **onOptionsItemSelected**. Each time a menu item is clicked or touched by the user, the Android runtime calls this function. Listing 3 shows a simple way to handle the event.

**listing 3. onOptionsItemSelected**
```kotlin
override fun onOptionsItemSelected(item: MenuItem?): Boolean {
  if(item?.itemId == R.id.menuFile) {
    showMessage(“File Menu “) // user defined function
    return true
  }
}
```

The Android runtime passes the **MenuItem** object each time it calls onOptionsItemSelected. We can use this object route program logic via if-else constructs. But using Kotlin's when might be more appropriate. See listing 4 for a complete code listing of hte MainActivity.

NOTE: I used the safe-call operator (?.)  in the expression **item?.itemId** because MenuItem is declared as a nullable type. There is a chance that it might be null.

**listing 4. MainActivity.Kt**
```kotlin
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import android.support.design.widget.Snackbar
import android.view.Menu
import android.view.MenuItem

import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
  }

  override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    menuInflater.inflate(R.menu.main_menu, menu)
    return super.onCreateOptionsMenu(menu)
  }

  override fun onOptionsItemSelected(item: MenuItem?): Boolean {

    when(item?.itemId) {
      R.id.menuFile -> {
        showMessage("File menu")
        return true
      }
      R.id.menuEdit -> {
        showMessage("Edit menu")
        return true
      }
      R.id.menuHelp -> {
        showMessage("Help menu")
        return true
      }
      R.id.menuExit -> {
        showMessage("Exit")
        return true
      }
    }

    return super.onOptionsItemSelected(item)
  }

  private fun showMessage(msg:String) {
    Snackbar.make(root_layout, msg, Snackbar.LENGTH_LONG).show()

  }
}
```

<strong>NOTE</strong>: This project used the Snackbar control. If you will follow this example, you need to add the  'com.android.support:design' in the dependencies entries of the **build.gradle** file (see this post for instructions http://bit.ly/usingsnackbar)

{% include book_las3_kotlin.html %}

