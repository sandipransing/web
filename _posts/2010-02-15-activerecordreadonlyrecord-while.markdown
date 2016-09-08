---
layout: post
title: "ActiveRecord::ReadOnlyRecord while updating object fetched by joins"
date: 2010-02-15T21:34:00+05:30
comments: false
categories:
 - rails
 - active record
---

#### ActiveRecord find with join options retrieves object as readonly
```
station = Station.find( :first, :joins => :call, :conditions => ["customer_id = ? and date(insurance_expiry_date) = ?", customer.id, insurance_expiry_date ] )
```
Readonly object cannot modified and hence below line raises "ActiveRecord::ReadOnlyRecord" error. 
```
station.update_attributes({ :customer_id => 12 })
```
If you have to write on read only object then you can pass following option to find query
```
:readonly => false
```
Now below find is permitted to do write on fetched object records. 
```
station = Station.find( :first, :joins => :call, :conditions => ["customer_id = ? and date(insurance_expiry_date) = ?", customer.id, insurance_expiry_date ], :readonly => false )
```
