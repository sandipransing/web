---
layout: post
title: "using modules and mixins in ruby"
date: 2010-04-28T18:11:00+05:30
comments: false
categories:
 - ruby
---

Before getting started with modules and mixins, lets first find out the need of module & mixins in OOP.

In object oriented programming languages multiple inheritance is basic paradigm (child class extends behavior of base class).

C++ supports multiple inheritance. Java does support same using interfaces.

In ruby language, multiple inheritance is achieved very easily using mixin of modules & classes and that makes ruby as powerful language.

Ruby has classes, modules. Modules can be included inside other classes (mixins) to achieve multiple inheritance.

## Modules

```ruby
  module Greeting
    # here below is the instance method
    # and that can be accessed using object of class ONLY
    def hi
      puts 'Guest'
    end
  end     
```
Module methods can be called using scope resolution operator (::) or dot operator (.)

```ruby
Greeting::hi

# => undefined method `hi' for Greeting:Module (NoMethodError)
```
It is obivous to have an error because we are calling class method and that is not present inside module.

## Mixins

Lets include greeting module inside person class

```ruby
class Person
  include Greeting

end

person = Person.new
person.hi #=> "Guest"
```
Lets see how module methods can be defined. It can be done using either self or class_name.

```ruby
module Greeting
  # here below is the instance method
  # and that can be accessed using object of class ONLY
  def hi
    puts 'Guest'
  end
  # Below are module methods
  # this methods can be invoked
  # using Greeting::hi
  # OR Greeting.hi
  def self.hi
    puts 'hi Sandip'
  end
  # here is one more way to declare
  # module methods
  def Greeting.bye
    puts 'Bye Sandip!'
  end
end
```
Lets invoke methods

```ruby
Greeting::hi #=> "hi Sandip"
Greeting.bye #=> "Bye Sandip!"
```
#### Extending class behavior using modules

```ruby
class Person  
  def self.included(base)
    base.extend Greeting
  end
end
```
Got easy ?? Cheers!
