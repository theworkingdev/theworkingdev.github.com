---
title: How to connect to Ubuntu from Windows using RDP
layout: post
categories: linux
permalink: /2010/10/how-to-connect-to-ubuntu-to-from.html
date:   2010-10-05 
---

1. Get the tsclient with the command sudo apt-get install xnest
2. Enable xdmcp. Edit the configuration file, it's located at etc/gdm/gdm.conf 
3. Uncomment RemoteGreeter in the daemon section. Delete the pound or sharp sign on theleft of RemoteGreeter
4. Find the xdmcp section and change the value of Enable key, set it to true
5. Log out of Ubuntu in order to restart the gdm. Altenatively, if you don't want to logout, from the Terminal, you can restart with this command `sudo /etc/init.d/gdm restart`
6. You can now connect from your Windows machine to an Ubuntu machine using Remote Desktop Protocol
 