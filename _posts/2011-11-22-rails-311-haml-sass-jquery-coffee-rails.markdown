---
layout: post
title: "rails 3.1.1 haml sass jquery coffee-rails devise twitter-bootstrap railroady heroku"
date: 2011-11-22T00:07:00+05:30
comments: false
categories:
 - rails
 - nginx
 - heroku
---
Install RVM first 

```
bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvminstaller)
```

rvm list known
```
# MRI Rubies
[ruby-]1.8.6-head
[ruby-]1.8.7[-p352]
[ruby-]1.9.3-head
ruby-head

# JRuby
jruby-1.2.0
jruby-head

# Rubinius
rbx-1.0.1
rbx-2.0.0pre

# Ruby Enterprise Edition
ree-1.8.6
ree-1.8.7-head
```

Install ruby 1.9.3
```
rvm install 1.9.3-head
rvm gemset create rails311
rvm use 1.9.3-head@rails311 --default
  
gem install rails -v3.1.1 --no-rdoc --no-ri
  
gem install heroku
gem install rb-readline
```

Create new rails project 
```
rails new cdc -m http://railswizard.org/b22092a4358bbebb3a46.rb -J -T
```

Above command will create rails app, bundle install, and Heroku Deployment
```
http://railsblank.heroku.com/ (production)
```

Local System nginx-passenger setup
```
gem install passenger
rvmsudo passenger-install-nginx-module
```
If you find pcre download error then make sure you libpcre-dev pkg installed on your system otherwise install it and re-run
```
sudo apt-get install libpcre3-dev
```

Nginx Configuration
```
http {
  passenger_root /home/sandip/.rvm/gems/ruby-1.9.3-head@rails311/gems/passenger-3.0.9;
  passenger_ruby /home/sandip/.rvm/wrappers/ruby-1.9.3-head@rails311/ruby;
  
  server {
    listen 80;
    server_name railsblank.local;
    root /home/sandip/railsblank/public;
    rails_env development;
    passenger_enabled on;
}
```

git source code can be found here
```
git clone git://github.com/sandipransing/rails_blank.git
```
