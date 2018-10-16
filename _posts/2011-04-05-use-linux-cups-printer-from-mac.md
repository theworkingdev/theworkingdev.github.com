---
title: Use Linux CUPS printer from OSX
layout: post
categories: linux
permalink: /2011/04/use-linux-cups-printer-from-mac.html
date:   2011-04-05 
---

Click the Apple logo at upper-left most corner of your desktop, click Print and Fax

On Printers column, click the plus sign to add a printer, there is a + sign at the lower left portion of the dialog window

Look for the Protocol settings. Click the dropdown and set it to "Internet Printing"

On the address field, type the IP address or the hostname of the CUPS server, just write the hostname, don't include the protocol. Hence you would write CupsIpAddress, not `ipp://CupsIpAddress`

On the queue input field, type the Queue name of the target printer attached to the CUPS server, the queue name of printers in a CUPS server is always prefixed by the word printers/. You need to ask your administrator what is the name of the printer as defined on the CUPS server. If you are the administrator, the name of the queue name is the same as what you typed on the name field of a printer when you were installing your printer to the CUPS server. What you need to type on the queue input field is `printers/nameofprinteroncups`

On the name field, type something that you can easily recognize, when you have several printers installed on your MAC, it helps if the printer names are easily recognizable.

On the "print using" field, choose "select a driver to use", and just choose your appropriate driver .

 