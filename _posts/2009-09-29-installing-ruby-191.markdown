---
layout: post
title: "Installing ruby 1.9.1"
date: 2009-09-29T19:05:00+05:30
comments: false
categories:
 - ruby
 - installation
---
Installing ruby `1.9.1` got very easy. Please follow these simple steps :
## Download
```
wget ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.1-p0.tar.gz
```
## Extract
```
tar -xvf ruby-1.9.1-p0.tar.gz
```
## Configure
```
cd ruby-1.9.1-p0
./configure
```
## Install
```
make
make test
sudo make install
```

Thats, it !
