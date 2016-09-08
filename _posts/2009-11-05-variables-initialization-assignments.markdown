---
layout: post
title: "Variables initialization, assignments and swapping in ruby"
date: 2009-11-05T18:02:00+05:30
comments: false
categories:
 - ruby
---

## Single line code to swap variables in ruby
```ruby
x,y = y,x
```

## Examples
```ruby
x = "Fun"
y = "Rails"

x,y = y,x
#=> ["Rails", "Fun"]

puts x
#=> "Rails"

puts y
#=> "Fun"
```

## Variable assignments in Ruby
Also Multiple variables assignments can be done in a single line
```ruby
a,b,c,d = 1,2,3,4
#=> [1, 2, 3, 4]
```

Here is interpretation of the above line
```ruby
puts "a is #{a}, b is #{b}, c is #{c}, d is #{d}"
#=> "a is 1, b is 2, c is 3, d is 4"
```

## Multiple assignments in ruby
```
a = b = c = d = 12
```
Here `a`,`b`,`c` and `d` has value 12
