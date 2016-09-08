---
layout: post
title: "How to get all associated models of rails model"
date: 2009-11-26T20:40:00+05:30
comments: false
categories:
 - associations
 - active record
---

I have Site Model and i wanted to find all associated models of Site model which are having belongs_to relationship.
After doing lot headache, here is the solution i found
## has_many models of Site Model
```
 Site.reflect_on_all_associations.map{|mac| mac.class_name if mac.macro==:has_many}.compact
=> ["Layout", "User", "SubmenuLink", "Snippet", "Asset"]
```
## belongs_to model of Site Model
```
Site.reflect_on_all_associations.map{|mac| mac.class_name if mac.macro==:belongs_to}.compact
=> ["User", "Page", "User"]
```
## To get all associations 
```
Site.reflect_on_all_associations.map{|mac| mac.class_name}.compact
=> ["Layout", "User", "Page", "User", "SubmenuLink", "User", "Snippet", "Asset"]
```

Hari, U rocks !
