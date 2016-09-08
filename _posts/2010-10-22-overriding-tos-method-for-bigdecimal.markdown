---
layout: post
title: "Overriding to_s method for BigDecimal instance"
date: 2010-10-22T19:31:00+05:30
comments: false
categories:
 - ruby
 - core-extensions
---
## Requirement
It was requirement to display decimal numbers which are having scale values present to be displayed in decimal format otherwise display them as integer.

## Expectated Output
```
12.23 => 12.23
12.00 => 12
```
While rendering any object on html page by default *to_s* method gets executed. So, i overwrote *to_s* method of BigDecimal class as below.

Anyone having better solution. Please reply with your solutions. Many thanks in advance!
Put below code in file *config/intializers/core_extensions.rb* 

```ruby
class BigDecimal
    alias :old_s :to_s
    def to_s
      return to_i.to_s if eql? to_i
      self.old_s
    end
  end
```
