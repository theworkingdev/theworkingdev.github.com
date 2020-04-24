---
title: adb server out of date
layout: post
categories: android
permalink: /2016/06/adb-server-out-of-date.html
date:   2016-06-02 
---

## adb server out of date - linux

When you encounter an adb server out of date in a Linux environment, that might be because there's more than one location in your machine where the Android tools are installed. This error happened to me on a Lubuntu 15, while I was using Genymotion (for the emulator) and Android Studio (for the IDE).

**Problem**. Cannot connect to adb server via the command line. Simple commands like `adb devices` produces the out of date error

```bash
adb server is out of date.  killing...
cannot bind 'tcp:5037'
ADB server didn't ACK
* failed to start daemon *
error:
```

### Possible solutions

**Do not use the Genymotion SDK**. When you launch genymotion, go to "Settings" then "ADB". Choose "Use custom SDK" then provide the path to your android sdk.

**Remove any android-tools** that may have been installed on your machine. You can check if the adb tool shows up on the `which` command.

```bash
ls -l which adb 
```

If it shows up in a place other than /path-to/android-sdk/platform-tools, then you might have a copy of adb that is not part of the android sdk distribution.  

You can try to remove the non-sdk copy of adb, perhaps using

```bash
sudo apt-get purge android-tools-adb android-tools-fastboot 
```

Alternatively, you can simply navigate to the directory of your android-sdk and invoke the adb command directly from there.

```bash
cd /path-to/android-sdk/platform-tools/
./adb services
./adb shell
```

Remember to replace **/path-to/android-sdk/** with your own path to the android sdk. If you are using the sdk that came with an Android Studio installation, chances are, the sdk is located **/home/your_username/Android/Sdk**. So use the following command

```bash
cd 
cd Android/Sdk/platform-tools/
./adb services
./adb shell 
```

That should fix it.

 