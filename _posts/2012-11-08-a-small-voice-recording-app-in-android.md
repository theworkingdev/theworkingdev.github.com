---
title: Small voice recording app in Android
layout: post
categories: android
permalink: /2012/11/a-small-voice-recording-app-in-android.html
date:   2012-11-08 
---

Voice capture and voice playback in Android is handled by two classes; android.media.MediaRecorder and android.media.MediaPlayer, for recording and playback respectively.

They are very straightforward to use. The usual sequence in using MediaRecoder and MediaPlayer is;

1. Create an instance of MediaRecorder
2. Set its audiosource to MIC
3. Set the filename where the audio file will be stored
4. Set the encoding
5. Call the prepare() method (this is a necessary step, otherwise you will get some IllegalStateException). This method throws an IOException, so you need to handle it appropriately with either a try-cath or throws mechanism
6. Call the start() method to begin recording
7. Call the stop(), then release() methods when you want to stop recording

The audio playback follows a similar pattern;

1. Create an instance of android.media.MediaPlayer
set the datasource for the MediaPlayer. This is the audio file that you recorded in steps 1-8
3. Call the prepare() method of the MediaPlayer object—this too can throw an IOException, so it needs to be handled using either try-catch or throws mechanism as well
3. Call the start() method to start playing the audio file

The source for this exercise is below. There is no main.xml because the UI was built programatically, instead of declaratively


```java
package com.workingdev;

import android.app.Activity;
import android.os.Bundle;
import android.os.Environment;
import android.widget.LinearLayout;
import android.widget.Button;
import android.content.Context;
import android.view.View;
import android.view.View.OnClickListener;
import android.media.MediaRecorder;
import android.media.MediaPlayer;
import java.io.IOException;


public class Recorder extends Activity {
  /** Called when the activity is first created. */
  @Override
  public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(new RView(this));
  }
}

class RView extends LinearLayout implements OnClickListener {

  Button btnstart = null;
  Button btnstop = null;
  MediaPlayer mp = null;
  MediaRecorder mr = null;
  String filename = null;

  RView(Context ctx) {
    super(ctx);

    filename = Environment.getExternalStorageDirectory().getAbsolutePath();
    filename += "/test.3gp";

    mr = new MediaRecorder();
    mp = new MediaPlayer();

    btnstart = new Button(ctx);
    btnstop = new Button(ctx);

    btnstart.setText("start");
    btnstop.setText("stop");

    btnstart.setOnClickListener(this);
    btnstop.setOnClickListener(this);

    btnstart.setId(1);
    btnstop.setId(2);

    setOrientation(LinearLayout.VERTICAL);

    addView(btnstart);
    addView(btnstop);

  }

  public void onClick(View view){
    if(view.getId() == 1) {
      // START BUTTON
      btnstart.setText("Holding ...");
      btnstop.setClickable(true);
      btnstart.setClickable(false);
      startRec();
    }
    else if(view.getId() == 2) {
      // STOP BUTTON
      btnstart.setClickable(true);
      btnstart.setText("start");
      btnstop.setClickable(false);
      stopRec();
    }
  }

  void startRec() {
   
    mr.setAudioSource(MediaRecorder.AudioSource.MIC);
    mr.setOutputFormat(MediaRecorder.OutputFormat.THREE_GPP);
    mr.setOutputFile(filename);
    mr.setAudioEncoder(MediaRecorder.AudioEncoder.AMR_NB);

    try {
     mr.prepare();
    }
    catch(IOException ioe){
     // Some Logcat here
    }
    mr.start();
  }

  void stopRec() {
    mr.stop();
    mr.release();

    try {
     mp.setDataSource(filename);
     mp.prepare();
   }
   catch(IOException ioe) {
     // some Logcat here
   }
   mp.start();

  }
}
```

**Permissions**

The code above will already compile (and run). If you do the first testing, it will run for a while but will immediately crash when you press the Start button. The MediaRecorder will not initialize properly and hence will hit an IllegalStateException when you start invoking the MediaRecorder methods. The Android runtime needs to allow your application to capture and record audio. App permissions are declared on the AndroidManifest.xml file, you will need to add the entry `<uses-permission android:name="android.permission.RECORD_AUDIO" />` to the manifest. Make sure also that you add the permissions entry out the `<application .../>` node. Here is the full entry of the AndroidManifest.xml for this exercise.

 
```xml
<?xml version="1.0" encoding="utf-8"?>

<manifest xmlns:android="http://schemas.android.com/apk/res/android"

      package="com.thelogbox"
      android:versionCode="1"
      android:versionName="1.0">
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <application android:label="@string/app_name" >
        <activity android:name="Recorder"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest> 
```

 