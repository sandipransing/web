---
layout: post
title: "Getting wired and wireless working on ubuntu 9.0.4"
date: 2009-12-14T15:39:00+05:30
comments: false
categories:
 - wireless
 - ubuntu
---
It might be the case that earlier wireless was working and then it stopped working

Follow the simple steps here....

**1. Restart networking**
```
sudo /etc/init.d/networking restart
```
**2. Disable the *"Support for Atheros 802.11 wireless LAN cards"* on `system/administration/Hardware Drivers`, and reboot your box**
**3. Restart**
```
sudo reboot
```
That's it, Cheers! Still having problem follow the instructions [here](http://www.funonrails.com/2009/10/getting-wireless-working-on-ubuntu.html)
