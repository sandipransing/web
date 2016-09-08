---
layout: post
title: "nginx and thin installation and configuration"
date: 2010-03-26T02:59:00+05:30
comments: false
categories:
 - rails
 - nginx
 - passenger
---

Install nginx server using following command

```
apt-get install nginx
```

Edit nginx configuration and add server block inside html block.&nbsp;

```
server {

    listen       80;
    server_name  boost;

    root /home/sandip/rails_app/public;

    location / {

        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_redirect off;

        if (-f $request_filename/index.html) {
            rewrite (.*) $1/index.html break;
        }

        if (-f $request_filename.html) {
            rewrite (.*) $1.html break;

        }

        if (!-f $request_filename) {
            proxy_pass http://thin;
            break;
        }
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {

        root   html;

    }

}
```

## Install thin server as gem

```
sudo gem install thin
Building native extensions.  This could take a while...
Building native extensions.  This could take a while...

Successfully installed eventmachine-0.12.10
Successfully installed thin-1.2.7

2 gems installed
```

## Install thin service
```
sudo thin install
Installing thin service at /etc/init.d/thin ...
mkdir -p /etc/init.d
writing /etc/init.d/thin
chmod +x /etc/init.d/thin
mkdir -p /etc/thin
```

## Configure thin to start at system boot
```
sudo /usr/sbin/update-rc.d -f thin defaults
```

Then put your config files in /etc/thin

```
sudo /usr/sbin/update-rc.d -f thin defaults
update-rc.d: warning: thin stop runlevel arguments (0 1 6) do not match LSB Default-Stop values (S 0 1 6)
 Adding system startup for /etc/init.d/thin ...

   /etc/rc0.d/K20thin -&gt; ../init.d/thin
   /etc/rc1.d/K20thin -&gt; ../init.d/thin
   /etc/rc6.d/K20thin -&gt; ../init.d/thin
   /etc/rc2.d/S20thin -&gt; ../init.d/thin
   /etc/rc3.d/S20thin -&gt; ../init.d/thin
   /etc/rc4.d/S20thin -&gt; ../init.d/thin
   /etc/rc5.d/S20thin -&gt; ../init.d/thin
```

Create thin configuration

```
sudo thin config -C /etc/thin/<config-name>.yml -c <rails-app-root-path> --servers <number-of-threads> -e <environment>
</environment></number-of-threads></rails-app-root-path></config-name>
```

In my case,

```
sudo thin config -C /etc/thin/rails_app.yml -c /home/sandip/rails_app --servers 3 -e production

&gt;&gt; Wrote configuration to /etc/thin/rails_app.yml
```

thin configuration file will look like

Start/stop/restart Nginx &amp; thin server using command

```
sudo service nginx start|stop|restart
sudo service thin start|stop|restart
```
