---
layout: post
title: "Dynamic creation of variables in ruby"
date: 2010-02-16T16:12:00+05:30
comments: false
categories:
 - ruby
 - meta-programming
---

## 1. To create local variables in ruby dynamically

Using eval method in ruby
```ruby
eval("local=4")
#=> 4 
p local
#=> 4
eval("local_#{1}=4")
#=> 4 
puts local_1
#=> 4
```
## 2. To create & get instance variables dynamically in ruby

`instance_varaiable_set` & `instance_varaiable_get` methods are provided and no need to do it using eval

```ruby
(0..3).each do |i|
  instance_variable_set("@instance_#{i}", i*i+1)
end
#=> 0..3 
@instance_0
#=> 1 
@instance_2
#=> 5 
```
## 3. Dynamic constant variable creation in ruby
```ruby
class Example
end

Example.const_set('A',200)
#=> 200 

Example::A
#=> 200
``` 
