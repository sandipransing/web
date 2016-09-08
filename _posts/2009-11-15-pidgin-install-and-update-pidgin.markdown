---
layout: post
title: "pidgin install and update pidgin messenger on ubuntu interpid"
date: 2009-11-15T12:57:00+05:30
comments: false
categories:
 - installation
 - ubuntu
---

## Fresh pidgin Installation
```
sudo apt-get update
sudo apt-get install pidgin
```

Earlier versions of pidgin has problem in connecting yahoo messenger which is solved in newer versions.
In order to update old version of pidgin, simply follow instructions written below and you are done.
New version provides video and voice chat, also buddy icon improvements are added.

## Add GPG key
```
sudo apt-get update
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys A1F196A8
```
Goto `System` > `Administration` > `Software Sources` and select `Third-Party Software tab` and click `ADD` and simply Copy-Paste the following Repo
```
deb http://ppa.launchpad.net/pidgin-developers/ppa/ubuntu intrepid main 
deb-src http://ppa.launchpad.net/pidgin-developers/ppa/ubuntu intrepid main
```
