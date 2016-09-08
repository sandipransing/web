---
layout: post
title: "ruby on rails installation on fresh ubuntu machine"
date: 2010-03-25T23:36:00+05:30
comments: false
categories:
 - ruby
 - installation
 - rails
 - ubuntu
---

Ruby On Rails Installation on fresh ubuntu machine

Installation steps are applicable to ubuntu versions interpid, karmic koala. Make necessary changes according to package manager provided by other linux operating systems in order install ruby and rails.

Before getting started to installations make sure to build essential packages on fresh ubutnu machine. Ubuntu machine has built in apt-get package manager and that i loves because it is very to use as compared to other OS. 
```
sudo apt-get install build-essential
```

Now there are many versions of ruby available including latest 1.9.2

But would like to prefer ruby version 1.8.7 as it is the most stable version as of now. I would like to recommend installtion of Ruby Enterprise Edition (REE) version as it is uses minimal system resources and consumes less memory. 


Instrunctions to setup normal ruby and rails environment

## 1. Install Ruby, Rails, MySQL, irb and neceesary packages using single command
```
sudo apt-get install ruby ri rdoc mysql-server libmysql-ruby ruby1.8-dev irb1.8 libdbd-mysql-perl libdbi-perl libmysql-ruby1.8 libmysqlclient15off libnet-daemon-perl libplrpc-perl libreadline-ruby1.8 libruby1.8 mysql-client-5.1 mysql-common mysql-server-5.1 rdoc1.8 ri1.8 ruby1.8 irb libopenssl-ruby libopenssl-ruby1.8 libhtml-template-perl mysql-server-core-5.1 libmysqlclient16 libreadline5 psmisc
```
## 2. Install ruby gems 
```
wget http://rubyforge.org/frs/download.php/60718/rubygems-1.3.5.tgz
tar xvzf rubygems-1.3.5.tgz
cd rubygems-1.3.5
sudo ruby setup.rb
```

## 3. Create symbolic links to installation paths 
```
sudo ln -s /usr/bin/gem1.8 /usr/local/bin/gem
sudo ln -s /usr/bin/ruby1.8 /usr/local/bin/ruby
sudo ln -s /usr/bin/rdoc1.8 /usr/local/bin/rdoc
sudo ln -s /usr/bin/ri1.8 /usr/local/bin/ri
sudo ln -s /usr/bin/irb1.8 /usr/local/bin/irb
```
## 4. Setup gemrc in order to avoid rdoc installation and to setup gem sources.

Add following lines to ~/.gemrc file 
```
---
gem: --no-ri --no-rdoc
:benchmark: false
:verbose: true
:backtrace: false
:update_sources: true
:sources:
- http://gems.rubyforge.org
- http://gems.github.com
:bulk_threshold: 1000
```

## 5. Install rails 
```
sudo gem install rails --no-rdoc --no-ri
```
## Ruby Enterprise Edition i.e. REE (ruby 1.8.7) Installation
This will install ruby, rails, mysql, nginx and passenger

#### 1. Download REE setup and install it 
```
wget http://rubyforge.org/frs/download.php/68719/ruby-enterprise-1.8.7-2010.01.tar.gz /opt/ruby

cd /opt
./ruby/installer
```
#### 2. Add ruby inside path 
```
vi ~/.bashrc
# Add following line at the end of file
export PATH=/opt/ruby/bin:$PATH
```
Thanks to @vwadhwani!
