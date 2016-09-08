---
layout: post
title: "ruby enumerable & to_proc (ampersond & symbol shortcut)"
date: 2012-02-23T02:09:00+05:30
comments: false
categories:
 - ruby
 - blocks
 - enumerable
---

Basically Enumerable mixin gives collection classes a variety of traverse, search, sort methods.

**Understanding ruby blocks**

> Blocks are statements of code written in ruby. one can take them as similar to c language macro's

**Different ways to define blocks**
```ruby
a = proc do
  puts "hello"
end
a.call #=> hello

b = lambda do |u|
  puts "hello #{u}"
end
b.call('sandip')#=> hello sandip

c = proc {|user| puts user }
c.call('sandip') #=> sandip
```
<!--more-->
**Passing block to enumerator**

Lets assume we have collection array of strings and we want to print it
```ruby
a = ['hi', 'sandip', 'how', 'you', 'doing', '?']
=> ["hi", "sandip", "how", "you", "doing", "?"]

a.each {|w| puts w }

q = proc {|w| puts w }
=> #<Proc:0x00007f9d2be13140@(irb):89>

a.each(&q) #=>
hi
sandip
how
you
doing
?

a.map{|r| q.call(r)} #=>
hi
sandip
how
you
doing
?
```

**Understanding symbol#to_proc**

> Symbol has method `to_proc` which converts symbol to block where symbol is taken as method to be executed on first argument of proc

** How to_proc got implemented inside Symbol class**
```ruby
class Symbol
  def to_proc
    Proc.new { |*args| args.shift.__send__(self, *args) }
  end
end
```
**Lets have some examples:**
```ruby
v = :even?.to_proc # equivalent to proc {|a| a.even?} 
#=> #<Proc:0x00007f9d2bddcb90@(irb):97> 
q = [1, 2, 3, 5, 67]  
q.map(&v) => [false, true, false, false, false]
```
 **Is there any shortcut?**
Yes, there is shortcut to have block passed to enumerators on the fly using ampersand followed by colon (i.e. symbol)
```ruby
q = [1, 2, 3, 5, 67]  
q.map(&:even?) <=> q.map(&:even?.to_proc)  
q.map(&:even?.to_proc) 
#=> [false, true, false, false, false]  
q.map(&:even?) 
#=> [false, true, false, false, false]
```
**Some handy examples**
```ruby
[1, 2, 3, 5, 67].inject(&:+) #=> 78 
[1, 2, 3, 5, 67].inject(:+) #=> 78 
[1, 2, 3, 5, 67].any?(&:even?) #=> true 
[1, 2, 3, 5, 67].detect(&:even?) #=> 2 
['ruby', 'on', 'rails'].map(&:upcase) #=> ["RUBY", "ON", "RAILS"]
```
