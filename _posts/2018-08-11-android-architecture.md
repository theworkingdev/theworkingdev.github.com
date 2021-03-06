---
title: Android architecture 
layout: post
categories: android, kotlin
permalink: /2018/08/android-architecture.html
date:   2018-08-11
image: /images/book_las3_kotlin.png
---

The most visible part of Android, at least for developers, is its operating system (OS). An OS is a complex thing but for the most part, it is what stands between a user and the hardware. That is an oversimplification, but it will suffice for our purposes. By "user", I don’t literally mean an end user or a person. What I mean by it is an application, a piece of code that a programmer creates, like a word processor or an email client.

 

Take the email app for example, as you type each character on the keys, the app needs to communicate to the hardware for the message to make its way to your screen, hard drive and eventually send it to the cloud via your network. It is a more involved process than I describe it here but that is the basic idea. At its simplest, an OS does three things

 
1. manages hardware on behalf of applications
2. provides services to applications like networking, security and memory management etc.
3. manages execution of applications, this is the part that allows us to run multiple applications, seemingly, almost at the same time.
 

Figure 1 shows the logical architecture of the Android platform.

![Android Architecture](/images/androidarchitecture.png) 
**Figure 1. Android’s logical architecture**


At the lowest level of the diagram is the Linux kernel. It’s responsible for interfacing with the hardware among other things. It’s also responsible for various services like memory management and execution of processes.

 

Linux is a very stable OS and is quite ubiquitous, you can find this OS in wide use. It can run on things as small as watches or as large as server farms. Android has an embedded Linux inside it which handles hardware interfacing and some other kernel functions.

 

On top of the Linux kernel are low level libraries like SQLite, OpenGL etc. These are not part of the Linux kernel but are still low level and as such, are written mostly in C/C++. On the same level, you will find the android runtime (android class libraries + dalvik virtual machine), which is where Android applications are run.


Next up is the application framework layer. It sits on top of both the low-level libraries and the android runtime because it needs both. This is the layer that we will interact with as an application developer because it contains all the libraries we need to write apps.


Finally, on top is the application layer. This is where all our applications reside, both the ones we write and the ones that comes prebuilt. It should be pointed out that prebuilt applications which comes with the device do not have any special privileges over the ones we will write. If you don’t like the email app of the phone, you can write your own and replace it. Android is democratic like that.

{% include book_las3_kotlin.html %}
 