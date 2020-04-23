---
title: Android Screen shooting with ddms
layout: post
categories: android
permalink: /2012/04/android-screenshooting-using-ddms.html
date:   2012-04-01 
---

1. You need a properly installed Android SDK setup before you can proceed.
Launch ddms from the command line. The path to the android executable has to be included in your PATH variable -- `ddms`
2. Enable USB debugging on the Android device, before you connect it to the PC (or Mac, or Linux). This is usually done by accessing Settings > Applications > Development, there is a check box beside "Enable USB debugging". Connect the Android device to your PC via a USB cable
3. The connected device should now be visible within the DDMS window. Select it, then
4. Press `CTRL-a` on your keyboard