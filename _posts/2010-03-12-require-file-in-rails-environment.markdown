---
layout: post
title: "require file in rails environment"
date: 2010-03-12T00:39:00+05:30
comments: false
categories:
 - rails
---
There are different ways to load particular file in rails application environment if the required file exists.

**1. Using RAILS_ROOT**
```
if File.exists?(file=File.join(RAILS_ROOT,'config', 'initializers', 'smtp_gmail.rb'))
  require file
end
```
**2. Using Rails.root &amp; join**
```
require Rails.root.join('config', 'initializers', 'smtp_gmail.rb')
```
**3. Using Rails.root, join &amp; exists? method**
```
require file if File.exists?(file = Rails.root.join('config', 'initializers','smtp_gmail.rb'))
```
**4. Using File and direct path to file**
```
require  File.join('root','app','config', 'initializers', 'smtp_gmail.rb')
```
Among all above methods of  loading file, Using `Rails.root`, `join &amp; exists?` method seems to be pretty good to have.
Any improvements are most welcome!
