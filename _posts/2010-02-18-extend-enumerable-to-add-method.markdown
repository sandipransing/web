---
layout: post
title: "Extend enumerable to add method collect_with_index"
date: 2010-02-18T14:24:00+05:30
comments: false
categories:
 - rails
 - core-extensions
---

Extend enumerable functionality to iterate along with index 

```ruby
module Enumerable
  def collect_with_index(i=0)
    collect{|elm| yield(elm, i+=1)}
  end
  alias map_with_index collect_with_index
end
```
#### Example use : 

```
 ['ruby', 'rails', 'sandip'].map_with_index{ |w,i|  [w, i] }
 #=> [["ruby", 1], ["rails", 2], ["sandip", 3]] 

 ['ruby', 'rails', 'sandip'].collect_with_index{ |w,i|  [w,  i] }
 #=> [["ruby", 1], ["rails", 2], ["sandip", 3]]

 #By default index starts from zero to specify custom index to start from,
 #pass index to collect_with_index

 ['ruby', 'rails', 'sandip'].map_with_index(-1){ |w,i|  [w, i] }
 #=> [["ruby", 0], ["rails", 1], ["sandip", 2]]

 ['ruby', 'rails', 'sandip'].map_with_index(5){ |w,i|  [w, i] }
 #=> [["ruby", 6], ["rails", 7], ["sandip", 8]]
```
