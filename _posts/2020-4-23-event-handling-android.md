---
title: Event Handling in Android 
layout: post
categories: 
  - java
  - android
comments: true
date:   2020-4-23   
author: "Ted Hagos"
---


The user interacts with an app by touching, clicking, swiping or typing something.  The Android framework captures, stores, processes and sends these actions to the app as event objects. The diagram (figure 1) below summarizes these interactions

![](/images/android-event-handling.png)
<figcaption>Figure 1. Android Event Handling Overview</figcaption>

You can make the app respond these to events by doing the following;

1. Create a listener object that's appropriate for the event you want the app to react to; if you want to respond to a click event, then use an the **OnClickListener** object. If you want to react to long clicks, you can use **OnLongClickListener**
2. Override the abstract methods of the listener. In case of the **OnClickListener**, the abstract method you need to override is the **onClick()** method
3. Bind the listener object to a View object, like a Button; Figure 2 shows an annotated diagram on how this might look in code

![](/images/android-event-model.png)
<figcaption>Figure 2. Android Event Model</figcaption>

Assuming that in your project, you already have a Button object **id** of which is _button_, the following code gets a reference to the Button object

```java
Button btn = (Button) findViewById(R.id.button); 
```

Next, we can create a listener object and bind the Button (**btn**) to that listener object, like this

```java
btn.setOnClickListener(new View.OnClickListener(){  
  @Override
  public void onClick(View view) {  
    // statement;
  }
});

```

The `onClick()` method, as shown in the code above, will be called whenever **btn** is clicked by the user; that's where you should put program logic.

View objects, like _Buttons_, can be bound to other listeners; the table below shows some of the more common listener interfaces.


**Table 1. Common listener interfaces**

| **Interface**                        | **Method**              | **Description**                                              |
| ------------------------------------ | ----------------------- | ------------------------------------------------------------ |
| **View.OnClickListener**             | `onClick() `            | This gets called when the user either  touches and holds the control (when in touch mode), or focuses upon the item  with the navigation keys then presses the ENTER key |
| **View.OnLongClickListener**         | `onLongClick()`         | Almost the same as a click, but only  longer                 |
| **View.OnFocusChangeListener**       | `onFocusChange()`       | when the user navigates onto or away from  the control       |
| **View.OnTouchListener**             | `onTouch()`             | Almost the same as click action but this  handler lets you find out if the user swiped up or down. You can use this to  respond to gestures |
| **View.OnCreateContextMenuListener** | `onCreateContextMenu()` | Android calls this when a ContextMenu is  being built, as a result of a sustained long click |





{% include book_learn_androidstudio4.html %}