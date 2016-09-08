---
layout: post
title: "Bugging around Active Support's Class.class_attribute extension"
date: 2011-12-28T19:14:00+05:30
comments: false
categories:
 - ruby
 - active-support
---
We all know Active Support library constantly keeps adding new extensions to ruby core library and hence rails framework.
Do you know now inside ruby class we can have class_attribute placeholder. 

```ruby
class A
  class_attribute :counter, :access_time
end

A.counter = 12
A.counter #=> 12
A.new.counter #=> 12
```

Inheritance
```ruby
class B < A
end

B.counter #=> 12
B.access_time #=> nil
B.access_time = Time.now
B.access_time #=> Wed Dec 28 18:55:06 +0530 2011
B.new.access_time #=> Wed Dec 28 18:55:06 +0530 2011
A.access_time = nil
```

Restricting instance from writing class_attributes 
```ruby
class V
  class_attribute :counter, :instance_writer => false
end

V.new.counter = 12
NoMethodError: undefined method `counter=' for #<#<Class:0x7f8f9d5c2fb8>:0x7f8f9d5b1038>
```
Other ways
```ruby
a_class = Class.new{class_atrribute :counter}

a_class.counter = 13
a_class.counter #=> 13
a_class.new.counter #=> 13

p = Class.new { class_attribute :help, :instance_writer => false }
p.new.help = 'Got a second!'
NoMethodError: undefined method `help=' for #<#:0x7f8f9d5b1038>
p.help = 'Got a second!'
p.help #=> "Got a second!"
```
