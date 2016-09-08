---
layout: post
title: "Simplified Active Record Extensions"
date: 2011-12-28T14:21:00+05:30
comments: false
categories:
 - rails
 - active record
 - pagination
---
Below extensions to active record base simplifies its usage while coding.

```ruby
# config/initializers/core_extensions.rb 
class ActiveRecord::Base

  def self.pagination(options)
    paginate :per_page => options[:per_page] || per_page, :page => options[:page]
  end

  def self.options_for_select(opts={})
    opts[:name] ||= :name
    opts[:attr] ||= :id
    opts[:prompt] ||= 'Please Select'
    all.collect{|c| [c.send(opts[:name].to_sym), c.send(opts[:attr].to_sym)]}.insert(0, [opts[:prompt], nil])
  end
end
```

Usage
```ruby
class Student< ActiveRecord::Base
 #attributes #=> name, age
end

## 
Student.pagination({:page => 10})
Student.pagination({:per_page => 2, :page => 2})
Student.options_for_select
Student.options_for_select({:prompt => 'Please select'})
Student.options_for_select({:name => :age})
```
