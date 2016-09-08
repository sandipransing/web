---
layout: post
title: "Eval method in ruby"
date: 2010-02-16T15:34:00+05:30
comments: false
categories:
 - ruby
 - meta-programming
---
Eval method in ruby executes string/expression passed as parameter. 

Example:
```ruby
eval("5+3") 
#=> 8   
eval("a=5")  
#=> 5   
eval("b||=a")  
#=> 5   
```
Its part of ruby meta-programming and not recommended approach unless there is no any alternative to do.
