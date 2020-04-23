---
title: How to sign an Android app
layout: post
categories: android
permalink: /2012/11/how-to-sign-android-app.html
date:   2012-11-02 
---

You have to sign your app before you can sell them and before other people can install them on their devices. Follow the steps below to sign your app.

**1. Generate a key**

You will need a key to sign your app. This can be generated using the keytool command. The keytool is part the Java SDK. If <JAVA_HOME>/bin folder is part of your system path, you should be able to invoke the keytool command from a terminal window

To generate a key, run the following command from a terminal window

```bash
keytool -genkey -v -keystore thelogbox_key.keystore -alias thelogbox_key_alias -keyalg RSA -keysize 2048 -validity 10000
```

**2. Use the jarsigner**

```bash
jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore my-release-key.keystore my_application.apk alias_name
```

Substitute the actual name of your application in place of my_application above. The jarsigner prompts you to provide passwords for the keystore and key. It then modifies the apk in-place and signs it accordignly. Note that you can sign an APK multiple times with different keys.

One thing to note is that as of JDK 7, the default signing algorithim has changed, requiring you to specify the signature and digest algorithims, -sigalg and -digestalg, when you sign an APK.

**3. Verify that your apk is signed**

```bash
jarsigner -verify my_signed.apk 
```

You can also use the command

```bash
jarsigner -verify -verbose my_application.apk
```

or this command

```bash
jarsigner -verify -verbose -certs my_application.apk
```

**4. Align the package**

Use the `zipalign` command on your apk file after you have signed it with your private key. This command ensures that all uncompressed data starts with a particular byte alignment relative to the start of the file, zipalign is part of Android SDK, not the Java SDK.

Ensuring the alignment at 4 byte boundaries has a positive effect on application performance when installed on a device.

```bash
zipalign -v 4 your_project_name-unaligned.apk your_project_name.apk
```

When aligned, the Android system is able to read files with mmap(), even if they contain binary data with alignment restrictions, rather than copying all of the data from the package. The benefit is a reduction in the amount of RAM consumed by the running application.
 