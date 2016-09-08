---
layout: post
title: "remote access, remote copy and files compress, decompress on linux"
date: 2009-11-06T18:12:00+05:30
comments: false
categories:
 - ubuntu
---

## Remote Login
`ssh` client is a program for logging into remote system and execute commands on that system

```
ssh [-l login_name ] hostname | user@hostname [command ]

# other options
ssh [-afgknqstvxACNTX1246 ] [-b bind_address ] [-c cipher_spec ] 
[-e escape_char ] [-i identity_file ] [-l login_name ] [-m mac_spec ] 
[-o option ] [-p port ] [-F configfile ] [-L port host hostport ] 
[-R port host hostport ] [-D port ] hostname | user@hostname [command ]
```
One can use putty for remote login through windows machine

## Remote Copy
`scp` - secure copy (remote file copy program) that copies file between computers over network

```
scp source[ source file path] destination[user@funonrails.com:/home]

# Other options
[-F ssh_config ] [-S program ] [-P port ] [-c cipher ] [-i identity_file ] 
[-o ssh_option ] [[user@ ] host1 : file1 ] [... ] [[user@ ] host2 : file2 ]
```

On windows system `pscp.exe` can be used for remote copy

## Compress
`tar` is a package that allows you to compress direcory or file
```
tar czfv Test.tar.tgz Test/
```

## Decompress
`gzip` is a package that allows to extract compressed file/directories.

```
gzip -dc target.tar.tgz | tar xf -
```
