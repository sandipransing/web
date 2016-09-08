---
layout: post
title: "Auto login remote server by copying ssh public key to authorized keys"
date: 2011-02-03T16:21:00+05:30
comments: false
categories:
 - linux
 - ssh
---

While working on remote machine we often get headache of entering password for ssh login, scp files from one server to another server. Adding public key as authorized_keys on remote server solves this problem. 
## Copying from my system to remote server
```
cat ~/.ssh/id_rsa.pub | ssh sandip@server 'cat >> ~/.ssh/authorized_keys'
```

## Copying from remote server to my system 
```
ssh server 'cat ~/.ssh/id_rsa.pub' | cat >> ~/.ssh/id_rsa_client.pub
```
