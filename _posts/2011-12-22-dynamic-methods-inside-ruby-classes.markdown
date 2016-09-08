---
layout: post
title: "Dynamic methods inside ruby classes"
date: 2011-12-22T17:54:00+05:30
comments: false
categories:
 - ruby
 - meta-programming
---
Ruby language is dynamic and robust. We can define methods inside ruby classes at runtime. 

```ruby
class A
  define_method :a do
    puts "hello"
  end
  
  define_method :greeting do |message|
    puts message
  end
end

A.new.a #=> hello
A.new.greeting 'Ram ram' #=> Ram ram
```

Can you imagine using dynamic methods below 24 lines of code is optimized to just 8 lines 
To know more on below code [read](http://www.funonrails.com/2011/12/multiple-resources-registrations-with)

### Before code
```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery
  helper_method :current_staff, :current_employee, current_admin

  def authenticate_staff!(opts={})
    current_staff || not_authorized
  end

  def current_staff
    current_user if current_user.is_a? Staff
  end

  def authenticate_employee!(opts={})
    current_employee || not_authorized
  end

  def current_employee
    current_user if current_user.is_a? Employee
  end
  
  def authenticate_admin!(opts={})
    current_admin || not_authorized
  end

  def current_admin
    current_user if current_user.is_a? Admin
  end
end
```

### After code
```ruby
# New Version using dynamic methods
%w(Staff Employee Admin).each do |k|
  define_method "current_#{k.underscore}" do
    current_user if current_user.is_a?(k.constantize)
  end
    
  define_method "authenticate_#{k.underscore}!" do |opts={}|
    send("current_#{k.underscore}") || not_authorized
  end
end
```
