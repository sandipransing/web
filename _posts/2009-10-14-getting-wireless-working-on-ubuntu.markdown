---
layout: post
title: "Getting wireless working on ubuntu machine"
date: 2009-10-14T21:53:00+05:30
comments: false
categories:
 - ubuntu
---
Pleae follow below steps to get wireless working on ubuntu.

## Disable wireless
Disable the `Support for Atheros 802.11 wireless LAN cards` on Hardware Drivers, and reboot your box.

## In a terminal:

#### Updates all package lists
```
sudo apt-get update
```
#### Update driver
```
sudo apt-get install linux-backports-modules-intrepid-generic
```
#### Reboot
```
sudo reboot!
```
