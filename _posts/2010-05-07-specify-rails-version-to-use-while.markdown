---
layout: post
title: "Specify rails version to new rails app"
date: 2010-05-07T18:40:00+05:30
comments: false
categories:
 - rails
---

On my development machine, I have rails versions installed ranging from rails `1.2.3` to `2.3`
How do i create new rails app that will use `rails 2.1.0`
Create new app with rails version `2.1.0`

```
rails _2.1.0_ simple_blog
```

Specify database to use while creating new rails app 
```
rails _2.1.0_ simple_blog -d mysql
```
## Comments

sitemap « Fun On Rails

[…] Specify rails version to use while creating rails project « Fun On Rails […]

sandipransing

specify database while creating rails application
```
rails demo -d 
rails _2.1.0_ my_rails_app -d mysql
```
