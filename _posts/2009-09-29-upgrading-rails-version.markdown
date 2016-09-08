---
layout: post
title: "upgrading rails version to rails 2.3.3"
date: 2009-09-29T19:12:00+05:30
comments: false
categories:
 - rails
 - upgrade
---

Here are the simple steps to be followed while upgrading rails version to use.

## Install rails version
Please install rails version 2.3.3 if not already installed

```
gem install rails 2.3.3
```

## Upgrade `environment.rb` for newer rails version
```
RAILS_GEM_VERSION = ‘2.3.3’ unless defined? RAILS_GEM_VERSION
```

## Upgrade necessary files for rails `2.3.3` version
Run below command to update necessary files for latest version

```
rake rails:upadte
```

## Start rails server
```
ruby script/server
```

Thats, it !
