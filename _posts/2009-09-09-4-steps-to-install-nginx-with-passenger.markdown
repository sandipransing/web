---
layout: post
title: "4 steps to install nginx with passenger"
date: 2009-09-09T23:47:00+05:30
comments: false
categories:
 - rails
 - nginx
 - passenger
---
## Install passenger program
```
sudo gem install passenger
```
## Install nginx server with passenger enabled
```
passenger-install-nginx-module
```

It will open `apt`, click `Enter` to contine then select option 1 for default install
then it will ask, *"Where do you want to install Nginx to?"*
```
Please specify a prefix directory [/opt/nginx]:
```
press enter then copy following block
```
server {
  listen 80;
  server_name www.yourhost.com;
  root /somewhere/public;   # <— be sure to point to ‘public’!
  passenger_enabled on;
}
```

## Make nginx Configuration
```
# vi /opt/nginx/conf/nginx.conf
# Make passenger_root and passenger_ruby path to configuration

http {
  passenger_root /usr/local/lib/ruby/gems/1.8/gems/passenger-2.2.4;
  passenger_ruby /usr/local/bin/ruby;
  ...
```
then add server configuration block inside http block
```
http {
  ...

  server {
    listen 80;
    server_name www.yourhost.com; //Make sure this dns entry inside /etc/hosts
    root /carsonline/public;   # <— be sure to point to ‘public’! //here carsonline is RAILS_ROOT
    passenger_enabled on;
  }
  ...
```
That's all, you are done !
## Launch Server

```
  /opt/nginx/sbin/nginx
```
