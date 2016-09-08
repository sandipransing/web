---
layout: post
title: "Best ways to populate dynamic array of numbers in ruby"
date: 2010-09-29T17:56:00+05:30
comments: false
categories:
 - ruby
 - array
---

#### Array of years using range
```ruby
((yr=Date.current.year)-9..yr).to_a
#=> [2001, 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010]
```

#### Array of years using lambda
```ruby
Array.new(10){|i| Date.current.year-i}
#=> [2010, 2009, 2008, 2007, 2006, 2005, 2004, 2003, 2002, 2001]
```

#### Array of months
```ruby
Date::MONTHNAMES.compact
#=> ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
```

#### Array of abbreviated months
```ruby
Date::ABBR_MONTHNAMES.compact
#=> ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
```

#### Array of abbreviated months with index (ps. collect_with_index is core extension method added to array)
```ruby
Date::ABBR_MONTHNAMES.compact.collect_with_index{|m, i| [m, i]}
#=> [["Jan", 1], ["Feb", 2], ["Mar", 3], ["Apr", 4], ["May", 5], ["Jun", 6], ["Jul", 7], ["Aug", 8], ["Sep", 9], ["Oct", 10], ["Nov", 11], ["Dec", 12]]
```
#### or

```ruby
Date::ABBR_MONTHNAMES.compact.each_with_index.collect{|m, i| [m, i+1]}
#=> [["Jan", 1], ["Feb", 2], ["Mar", 3], ["Apr", 4], ["May", 5], ["Jun", 6], ["Jul", 7], ["Aug", 8], ["Sep", 9], ["Oct", 10], ["Nov", 11], ["Dec", 12]]
```
