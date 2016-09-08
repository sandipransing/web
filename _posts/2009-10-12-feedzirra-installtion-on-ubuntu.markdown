---
layout: post
title: "Feedzirra installtion on ubuntu interpid"
date: 2009-10-12T20:36:00+05:30
comments: false
categories:
 - installation
---
Feedzirra is a feed library that is designed to get and update many feeds as quickly as possible.
This includes using `libcurl-multi` through the `taf2-curb` gem for faster `http gets` and `libxml` through `nokogiri` and `sax-machine` for faster parsing.

## Installing packages
```
sudo apt-get install libcurl3 libxml2 libxml2-dev libxslt1-dev
```
## Gem install
```
sudo gem install pauldix-feedzirra
```
