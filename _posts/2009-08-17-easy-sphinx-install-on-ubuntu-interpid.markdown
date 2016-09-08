---
layout: post
title: "Easy sphinx install on ubuntu interpid"
date: 2009-08-17T13:02:00+05:30
comments: false
categories:
 - thinking sphinx
---

#### Download `sphinx source` from website
```
sudo apt-get install libmysqlclient15-dev
wget http://sphinxsearch.com/downloads/sphinx-0.9.8-rc2.tar.gz
```
#### Extract source code
```
tar xvf sphinx-0.9.8-rc2.tar.gz && rm sphinx-0.9.8-rc2.tar.gz
```
#### Configure
```
cd sphinx-0.9.8-rc2
./configure
```
#### Install
```
make
sudo make install
```
