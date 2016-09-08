---
layout: post
title: "Copy database to another database through command"
date: 2010-02-16T16:53:00+05:30
comments: false
categories:
 - mysql
 - ubuntu
---

## 1. Copy One database to another database on same host
```
mysqldump -uroot -p  | mysql -uroot
```
## 2. Copy database to another database on remote host
```
mysqldump -uroot -p  | ssh host2 "mysql -uroot"
```
