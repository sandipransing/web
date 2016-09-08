---
layout: post
title: "How to get started with tata photon on ubuntu machine"
date: 2011-07-05T14:06:00+05:30
comments: false
categories:
 - ubuntu
---

In order to get tata photon working on ubuntu machine you need to install following packages 

**1. usb-modeswitch-data 2. usb-modeswitch**

Either you can download them from ubuntu site and install locally or install using apt manager 

Download links
```
http://packages.debian.org/squeeze/usb-modeswitch-data
http://packages.debian.org/squeeze/usb-modeswitch
```

apt installation
```
sudo apt-get install usb-modeswitch-data usb-modeswitch usb-modeswitch
```

After successful installation,

1. Connect photon usb card
2. Execute following commands one after another

```
sudo usb_modeswitch -c /etc/usb_modeswitch.d/12d1\:1446
wvdial
```

..and you should get connected via photon!
