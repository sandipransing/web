---
layout: post
title: "Programming ruby Linux basics"
date: 2010-03-02T12:29:00+05:30
comments: false
categories:
 - linux
 - ubuntu
---

## Remove svn files inside directory
```
rm -rf `find . -type d -name .svn
```

## Set path in linux
```
export PATH=$PATH:/usr/ruby/bin
```

Here /usr/ruby/bin is the path to ruby extecuatble. 
```
grep and print process pid
ps -ef | grep search_string | grep -v grep | awk '{print  $2 }'
```

## Kill process
```
kill -9 process_id_here
ps -ef | grep search_string | grep -v grep | awk '{print  $2 }' | xargs kill -9
```

## Store pid of process in a variable for further process
```
tokill=`ps -ef|grep ruby|grep -v grep|awk '{print $2}'`;kill -9 $tokill;
```
## Mirror a website

Command to clone/copy a website for offline/local browsing.
```
wget -mk www.funonrails.com
```
Above command downloads all website pages in depth level with stylesheets, images and javascripts.
