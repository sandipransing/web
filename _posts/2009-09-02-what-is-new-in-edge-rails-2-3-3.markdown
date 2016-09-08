---
layout: post
title: "What is new in edge rails 2.3.3 ?"
date: 2009-09-02T17:54:00+05:30
comments: false
categories:
 - rails
---
Rails community has released rails 2.3.3 recently with major upgrades and several bug fixes added.

On the other hand, `rails 3` ( i.e. merge of rails and merb framework ) is supposed to be officially get released in this May.

Lets see what are the notable features added in rails 2.3.3

## 1. Rack

Rack is an abstraction built on the top of rails framework

It provides minimal interface between webservers supporting ruby ( like  apache-mongrel, nginx-passenger ) and rails framework.
Gives access to middleware goodness i.e. wrapping HTML requests and response in simple way
unifies and distills the API for web servers, web frameworks, and software in between

[read more …](http://guides.rubyonrails.org/rails_on_rack.html)

## 2. Metal
It is a subset of rack middleware specially designed to integrate with rails application.

Whenever you need achieve raw speed for certain requests by skipping action controller stack.

[read more …](http://weblog.rubyonrails.org/2008/12/17/introducing-rails-metal)

## 3. Engines

It was a plugin that got merged in rails core.

It is used for sharing controllers, models and views seamlessly from within a plugin to your rails application.

[read more …](http://rails-engines.org/news/2009/02/02/engines-in-rails-2-3/)

## 4. Templates

These are the ruby files which describes which gems, plugins and initiliazers  has to be added while creating new rails project.

[read more …](http://rails-engines.org/news/2009/02/02/engines-in-rails-2-3/)

## 5. Nested Forms
Earlier nested forms for has_one and has_many associations wasn’t there in rails.
this release added nested forms complex associations in models.

[read more here…](http://weblog.rubyonrails.org/2009/1/26/nested-model-forms) and [also see here …](http://ryandaigle.com/articles/2009/2/1/what-s-new-in-edge-rails-nested-attributes)

## Edge rails Installation

```
gem install rails --source http://gems.rubyonrails.org
```

I am  always thankful to rails community for continuous improvements to rails core.
Cheers, now rails is getting its way !
