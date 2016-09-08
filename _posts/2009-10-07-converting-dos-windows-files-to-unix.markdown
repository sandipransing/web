---
layout: post
title: "Converting dos (windows) files to unix files"
date: 2009-10-07T18:27:00+05:30
comments: false
categories:
 - linux
 - dos2unix
---

While editing files on windows system,  we get `^M` characters added to files and those gets displayed when you open files on `unix` machines.
To remove such `^M` characters,

## Install dos2unix package
```
sudo apt-get install sysutils
```
## Convert individual file

```
dos2unix file_path
```
## Convert files recursively

Run following command to convert recursively all the files inside directory.

```
find . -type f -exec dos2unix {} \;
```
