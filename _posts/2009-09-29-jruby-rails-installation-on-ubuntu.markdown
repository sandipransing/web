---
layout: post
title: "jruby rails installation on ubuntu interpid"
date: 2009-09-29T21:22:00+05:30
comments: false
categories:
 - ruby
 - installation
---
## Installations

Install mysql if you dont have already installed

```
sudo apt-get install mysql-server mysql-client libhtml-template-perl mailx dbishell libcompress-zlib-perl mysql-doc-5.0 tinyca
```
Install ant and jdk
```
sudo apt-get install ant sun-java6-jdk
```
Install jruby
```
mkdir software
cd software
wget http://dist.codehaus.org/jruby/jruby-bin-1.1.5.tar.gz
tar xvfz jruby-bin-1.1.5.tar.gz
ln -s jruby-1.1.5 jruby
export PATH=$PATH:$HOME/software/jruby/bin
```
Install the version of rails wanted by jruby
```
jruby -S gem install jruby-openssl
jruby -S gem install rails
```
#### List your gems
```
jruby -S gem list
```
#### Install required gems
```
jruby -S gem install activerecord-jdbc-adapter activerecord-jdbcmysql-adapter
```
#### Create a rails app using mysql DB
```
cd ~/scripts/ruby
jruby -S rails wherehaveyoubeen -d mysql
cd wherehaveyoubeen
jruby script/generate controller states index
```
#### Configure access to the database server

```yml
# vi config/database.yml

development:
 adapter: jdbcmysql <- very important!
 encoding: utf8
 database: test_development
 username: root
 password: database
 socket: /var/run/mysqld/mysqld.sock
```
#### Generate DB
```
jruby -S rake db:create:all
jruby -S rake db:migrate
```

#### Edit your app
```
sudo apt-get install emacs ruby-elisp irb1.8 emacs22-el
emacs -bg black -fg wheat app/views/states/index.html.erb
```
#### Uncomment the secret in app/controllers/application.rb
```
vi app/controllers/application.rb
# remove secret from this file
```
### Run Server
```
jruby script/server
```
## Comments

**@Parag**

Very helpful article. Please keep up the great work! :-)
