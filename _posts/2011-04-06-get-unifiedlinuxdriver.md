---
title: Install Samsung CLP-315 on Linux
layout: post
categories: linux
permalink: /2011/04/get-unifiedlinuxdriver.html
date:   2018-08-01 
---


Get the UnifiedLinuxDriver.tar either from the CD that came with the printer or from the Samsung site. You need to navigate the site a little bit. Go to Support and Downloads page 
Find a way to upload UnifiedLinuxDriver.tar.gz to your headless server. FTP or SCP or even Samba shares should do it

Untar the driver file. 

```bash
sudo tar -xzvf UnifiedLinuxDriver.tar.gz
```

Change directory to cdroot, this is one of the folders in the UnifiedLinuxDriver folder
Run the installer 

```bash
sudo install.sh
```

The installer needs to run in GUI workstation, so it will give you some warnings that may look like errors. It will eventually detect that you are not running on a GUI and it will degrade gracefully to a text-based installation. We need to run the installer because we need a very specific file. That file is rastertosamsungsplc. We need the PPD file in order for the CLP 315 printer to function normally in Linux 
Next, copy the rastertosamsungsplc PPD file to the `/usr/lib/cups/filter` folder

```bash
cd /Path/To/Where/rastertosamsungsplc-is/
cp rastertosamsungsplc /usr/lib/cups/filterrastertosamsungsplc
sudo /etc/init.d/cupsd restart
```

To add the CLP 315 to a CUPS server, open a browser and go to http://CUPSServerName:631, go to Administration > Add Printer 