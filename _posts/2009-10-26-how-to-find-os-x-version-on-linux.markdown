---
layout: post
title: "How to find OS X version on linux machine"
date: 2009-10-26T23:35:00+05:30
comments: false
categories:
 - linux
 - general
---
## Command
Here is the command which will give you OS name with version along with other detail information.
```
cat /etc/*release*
```

## Example
On my ubutnu machine, It showed me :
```
cat /etc/*release*

DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=8.10
DISTRIB_CODENAME=intrepid
DISTRIB_DESCRIPTION="Ubuntu 8.10"
```
