---
layout: post
title: "nginx passenger configuration for rails application"
date: 2010-02-15T21:43:00+05:30
comments: false
categories:
 - rails
 - nginx
 - passenger
---
```
#user  nobody;
user www-data;
worker_processes  2;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    passenger_root /var/lib/gems/1.8/gems/passenger-2.2.8;
    passenger_ruby /usr/bin/ruby1.8;
    passenger_max_pool_size 3;

    include       mime.types;
default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
 server {
     listen 80;
     server_name localhost;

     root /home/josh/current/public;   # <--- be sure to point to 'public'!
     passenger_enabled on;
     passenger_use_global_queue on;
   }

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;
        #access_log  logs/host.access.log  main;
        
        location / {
            root   html;
            index  index.html index.htm;
        }
     #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
   }
}
```
