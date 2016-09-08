---
layout: post
title: "DelayedJOb's (DJ) fancy methods"
date: 2010-10-06T19:39:00+05:30
comments: false
categories:
 - delayed-job
 - rails
---

Delayed Job provides `send_later` and `send_at` as *instance* as well as *class*  methods. It also gives us `handle_asynchronously` as *class* method to be written inside class.
```ruby
module Delayed
  module MessageSending 
    def send_later(method, *args) 
      Delayed::Job.enqueue Delayed::PerformableMethod.new(self, method.to_sy m, args) 
    end 

    def send_at(time, method, *args) 
      Delayed::Job.enqueue(Delayed::PerformableMethod.new(self, method.to_sy m, args), 0, time) 
    end 

    module ClassMethods 
      def handle_asynchronously(method) 
        aliased_method, punctuation = method.to_s.sub(/([?!=])$/, ''), $1 
        with_method, without_method = "#{aliased_method}_with_send_later#{pu nctuation}", "#{aliased_method}_without_send_later#{punctuation}" 
        define_method(with_method) do |*args| 
          send_later(without_method, *args) 
        end 
        alias_method_chain method, :send_later 
      end 
    end
  end
end
```

## Usage of *send_later*, *send_at* and *handle_asynchronously*
```ruby
# instance method
user.send_later(:deliver_welcome)

# class_method
Notifier.send_later(:deliver_welcome, user)

Notifier.send_at(15.minutes.from_now, :deliver_welcome, user)

# Inside User class write below line after deliver_welcome method
handle_asynchronously :deliver_welcome
```
