---
layout: post
title: Dynamic conditions to rails associations
date: 2012-10-25T16:12:00+05:30
comments: false
---
We all know that rails models associations gets defined while class definitions are loaded and once defined can't be changed.

But still you can make use of `block parameter` to conditions to have `dynamic query conditions` inside associations.

Below line explains **how to** define dynamic associations :
```ruby
has_one :code_sequence, :class_name => 'Sequence', :conditions => 'kind = "#{self.kind}"'
```
Please make a note that below code will not work :
```ruby
has_one :code_sequence, :class_name => 'Sequence', :conditions => proc { |c| ['kind = ?', c.kind] }
```
