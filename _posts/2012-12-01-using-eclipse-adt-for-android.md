---
title: Using Eclipse ADT for Android
layout: post
categories: android
permalink: /2012/12/using-eclipse-adt-for-android.html 
date:   2012-12-01 
---

Make sure you have already installed the Android SDK and that you have properly configured the PATHs to tools and platform-tools. Make sure also that you have properly installed and configured Apache ANT.

Download Eclipse from eclipse.org, This is usually zipped or tgz'ed file. You will need to extract eclipse to a folder of your choice and setup the correct links/PATH

**Get the ADT plugin**

1. Launch Eclipse
2. Go to Help → Install new software
3. Click the Add button
4. On the Location text field, type _https://dl-ssl.google.com/android/eclipse/_, you can type whatever you want on the Name textfield, but I suggest that you name it "Android DT" or something descriptive
5. Click Ok then follow the prompts, it is straightforward from there.

Eclipse will restart after the plugin has been completely downloaded
 

**Create a test project**

Go to **File → New Project → Android → Android application project**. It will be a series of dialog screens, but what you need to remember are the following;

1. Application Name. Choose a name for this. This is an Eclipse construct, it needs it to manage projects
2. Project name. Put whatever you like
3. Package name. This is usually a reverse DNS notation of your domain name. The domain does not have to exist, so you can put whatever you like e.g. com.thelogbox.HelloWorld
4. Minimum required SDK. The lowest version of Android that your application is guaranteeing to run on; be careful here, the older release of Android is not guaranteed to be compatible with the newer versions of Android. Unless you absolutely know what you are doing, put exactly the same values on all the SDK fields in this dialog screen
5. On the next screen, you will have an option to create an Activity, leave it on if you want to have a default Activity class when the project is created
6. Follow the prompts to fill out some more project details


**Test the app**

If you don't have a physical Android device, you can use the AVD (android virtual device) but I strongly suggest that you get a physical device because the AVD is painfully slow. You need to get the AVD setup first before you can run anything, there are detailed instructions and information about the AVD on [android developer site](http://developer.android.com/tools/devices/managing-avds.html). You will need either the AVD or a physical device because Android applications are meant to run on an environment that is very different from your PC, that is why you need at least an emulator.

If you have a physical Android device, there are somethings you need to note before you can use it.

1. Enable the USB Debugging option on the physical device itself. This procedure will differ from one device brand to another, so you really need to know how to work this for your specific device brand
2. If you are using OSX, then you can stop here, you all setup. All you've got to do is to plug your device into the USB of the PC, and off you go. If you are using Windows or Linux, read on to step no. 3
3. Configure the USB settings on your machine before you can plug in your Android device and use it for development. There are detailed instructions on the android developer site, read through it and follow it
4. Once the setup is done, as soon as you click Run on the eclipse menu (CTRL F11), the new android project will be deployed to the physical machine
 