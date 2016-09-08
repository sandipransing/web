---
layout: post
title: "Managing multiple ruby versions with RVM"
date: 2009-09-30T18:32:00+05:30
comments: false
categories:
 - ruby
 - rvm
 - ubuntu
---

We often work on many projects at the same time and each may using different ruby and rails version.
Now, the big problem arises that how to manage multiple ruby version on the same system.

We know that managing multiple rails versions on same system is not a problem at all but what about ruby versions?
How one can manage multiple ruby versions? 
Also, while upgrading or degrading ruby version we need to install gems all again including rails.
i know this is a fucking pain in ass.. isn't it?

Lets see, How we can overcome with this headache. meantime there also can be some incompatibilities between two different ruby and rails versions.

Yesterday, i did some research and came to know that...

1.  ruby 1.8.7 and rails 2.3.3 got quite stability and effectively. we can use it in next developments.
2.  Also, after installing ruby 1.9.1 and dependent gems for my project, and wondering that there are many incompatibilities while installing new gems with ruby 1.9.1

Some of the gems are mysql, ferret, acts_as_ferret, erubies, etc.

Today, while google, i found `rvm` gem and that seems very nice to manage multiple ruby versions.

Here are some steps explaining how to use it.

## Install rvm gem to manage multiple ruby version
Before install make sure you already have ruby and rubygems installation
```
sudo gem install rvm
```

## Install rvm for a particular user
It will also show how to use gem
```
rvm-install
```
Now, Open new shell and show all ruby & jruby installations
```
rvm list
```

## Installing ruby using rvm
Specify ruby version
```
# rvm install RUBY_VERSION_TO_BE_INSTALLED
rvm install 1.9.1
```

## Install jruby
By default it will install jruby-1.3.1
```
rvm install jruby
```

## Specify which ruby version to use on shell
Note: You should have specified version installed via rvm
```
# rvm use RUBY_VERSION_TO_USE 
rvm use 1.9.1
```

## Default i.e. version before rvm gem installation
```
rvm use default
```
