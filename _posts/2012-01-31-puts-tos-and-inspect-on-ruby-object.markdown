---
layout: post
title: "puts, to_s and inspect on ruby object"
date: 2012-01-31T01:07:00+05:30
comments: false
categories:
 - ruby
---
`puts` converts ruby object into string by invoking to_s method on object. The default to_s prints the object's class and an encoding of the object id. In order to print human readable form of object use inspect
```ruby
locs = Location.find_by_sql('select * from locations')
Location Load (0.5ms)  select * from locations
```
Puts Object internally invokes to_s method on object to print
```ruby
locs.each do |l|
  # it calls to_s method on object
  puts l
end

#<Location:0x000000055bb328>
#<Location:0x000000055bb058>
```
<!--more-->

puts object followed by subsequent invoke of inspect method outputs readable object
```ruby
locs.each do |l|
  puts l.inspect # prints actual object
end

#<Location id: 15, name: "Annettaside3", street: "71838 Ritchie Cape", city: "East Destanystad", state: "Utah", zip: "58054", phone: 123456, other_phone: 987654, staff_strength: 40, is_active: true, created_at: "2012-01-25 11:17:26", updated_at: "2012-01-25 11:17:26", country_name: "Korea">
#<Location id: 16, name: "Sporerbury4", street: "73057 Jerad Shoal", city: "South Kyliefurt", state: "Delaware", zip: "46553-3376", phone: 123456, other_phone: 987654, staff_strength: 40, is_active: true, created_at: "2012-01-25 11:24:48", updated_at: "2012-01-25 11:24:48", country_name: "Australia">
```
