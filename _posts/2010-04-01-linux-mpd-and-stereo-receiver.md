---
title: Linux mpd and stereo receiver
layout: post
categories: linux
permalink: 2010-04-linux-mpd-and-stereo-receiver.html
date:   2010-04-01 21:14:28 +0800 
---

This project will let you control a music player daemon running on a Linux box. The Linux box (headphone jack) will be connected into a stereo receiver (input. It's an over engineered music player. But if you don't have anything better to do with an old notebook, this might save it from the junk yard.

For this project, I used the following;

* an old notebook with WiFi still working
* an old stereo receiver with 3.5mm stereo jack input 

Installation and Configuration

1. Get the MPD (music player daemon)  -- sudo apt-get install mpd mpc
2. Edit the configuration file located at /etc/mpd.conf
3. Find the entry on the configuration file where you are supposed to point to where you music files are
4. Save the config file
5. Restart the Music Player Daemon -- sudo /etc/init.d/mpd restart
6. Create the database for the first time to add all songs to the playlist -- sudo mpd --create-db then sudo mpd add l
7. Install MPD clients on your other computers. At the time I wrote this, I used ncmpc. Lots of things has changed since then, Wikia.com has a list of MPD clients