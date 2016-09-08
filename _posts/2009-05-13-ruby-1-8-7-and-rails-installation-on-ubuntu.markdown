---
layout: post
title: "Ruby 1.8.7 and rails installation on ubuntu interpid"
date: 2009-05-13T20:02:00+05:30
comments: false
categories:
 - rails
---
Execute following commands in order to install ruby and rails
```
sudo apt-get install build-essential
```
Below command will install all necessary packages. if you don't want any `package` from list then remove it from command.
```
sudo apt-get install ruby ri rdoc mysql-server libmysql-ruby ruby1.8-dev irb1.8 libdbd-mysql-perl libdbi-perl libmysql-ruby1.8 libmysqlclient15off libnet-daemon-perl libplrpc-perl libreadline-ruby1.8 libruby1.8 mysql-client-5.0 mysql-common mysql-server-5.0 rdoc1.8 ri1.8 ruby1.8 irb libopenssl-ruby libopenssl-ruby1.8 libterm-readkey-perl psmisc
```
## Gem Installation
Please find the latest stable gem version on rubyforge.
```
wget http://rubyforge.org/frs/download.php/45905/rubygems-1.3.1.tgz
tar xvzf rubygems-1.3.1.tgz
cd rubygems-1.3.1
sudo ruby setup.rb
```
And last to install rails without documentation
```
sudo gem install rails --no-rdoc --no-ri
```
and finally rails server, you can either install `apache mongrel` or `nginx + thin` whichever you feel suitable.

## Mongrel install
```
sudu gem install mongrel
```
