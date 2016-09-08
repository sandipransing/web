---
layout: post
title: "dynamic & bounded parameters and named routes"
date: 2012-01-31T02:43:00+05:30
comments: false
categories:
 - rails 3
 - routes
---
Rails routes can be customized to include your own parameters inside it. Lets first understand how named routes works:

## Adding dynamic parameters to routes 

Here exact parameters are matched to route and presence of each parameter is mandatory in order to construct urls. If blank parameter is supplied then it will raise `RoutingError` exception. 

## Exact matched named route declared as :

```ruby
match ':a/:b/:c', :to => 'home#index', :as => :q
```
#### rails console :
```ruby
app.q_url(:a, :b, :c)
#=> "http://www.example.com/a/b/c"

app.q_url(:a, :b, '')
#=> ActionController::RoutingError: No route matches {:controller=>"home", :a=>:a, :b=>:b, :c=>""}
```
<!--more-->
## Bound parameters to named routes
If you are sure that certain parameters can be blank then you can define them as optional parameter inside route

```ruby
match ':a/:b(/:c)', :to => 'home#index', :as => :q
```
#### rails console :
```ruby
app.q_url(:a, :b, '')   
#=> "http://www.example.com/a/b?c="

app.q_url(:a, :b)   
#=> "http://www.example.com/a/b" 
```
