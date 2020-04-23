---
title: Photoviewer in Android using implicit Intents
layout: post
categories: android
permalink: /2012/10/photoviewer-android-using-implicit-intents.html
date:   2012-10-01 
---

Photo Viewer in Android using Implicit Intents
This is small exercise on how to create a photo viewer app that responds to implicit Intents.

What we want to do is to create an app that can;

* Respond to an implicit Intent
* The implicit Intent has some data in it;
* Specifically, this data is image data, and we want our app to be one of the choices for the user when he opens a photo

When the user browses the picture gallery and picks a photo, our app will appear as one the apps that can be used for opening the photo.

We will use an Android emulator for this exercise, so we need to put some pictures on the emulator.

Start an emulator, then push some images to the sdcard. You can do this using the android device monitor. Navigate to the file browser, select /mnt/sdcard by pointing and clicking to it using your mouse. You can drag and drop images onto that folder

![](/images/android-device-monitor.png)

On some situations, some emulators may have a read-only filesystem. You won't be be able to push files to it until you have reconfigured the permissions of the `/mnt/sdcard` folder. You can change permissions to the sdcard folder using the adb shell, remember that Android is essentially running in Linux, so you can use the following Linux commands

```bash
adb shell                     # opens the adb shell
su
mount -o rw,remount rootfs /
chmod 777 /mnt/sdcard         # standard chmod command
exit
```

Launch the picture gallery application on the emulator. The pictures you just uploaded may not be visible immediately on the gallery. You can shutdown the emulator and restart it or alternatively, you can send a broadcast message to the emulator in order to rescan the sdcard for new content. You can use the adb shell to do that as well.

adb shell am broadcast -a android.intent.action.MEDIA_MOUNTED -d file:///sdcard
Create a new project with an empty activity. On the View pallette, get an ImageView control and place it on the main activity screen.

Handle the incoming intent inside the onCreate callback method.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);

  ImageView imageView = (ImageView) findViewById(R.id.imageView);

  Bundle b = getIntent().getExtras();
  if(b != null) {
    Uri uri = (Uri) b.get(Intent.EXTRA_STREAM);
    imageView.setImageURI(uri);
  }
}
```

Declare the intent in the android manifest file.

```xml
<intent-filter>
  <action android:name="android.intent.action.SEND"/>
  <category android:name="android.intent.category.DEFAULT"/>
  <data android:mimeType="image/*" />
</intent-filter>
```


 