---
layout: post
title: "Fix/Solution for NoMethodError? (undefined method `controller_name' for nil:NilClass)"
date: 2010-02-16T20:32:00+05:30
comments: false
categories:
 - rails
---

This error appears when same action gets called twice. There might be chances to have controller with same name twice inside app as well as plugins

In my case the error appeared while migrating from `rails 2.1` to `2.3`
Application was having `application.rb` and `application_controller.rb`

Files deleted:
`trunk/app/controllers/application.rb`

And that solved problem !
