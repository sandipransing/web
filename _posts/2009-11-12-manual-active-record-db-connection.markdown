---
layout: post
title: "Manual active record db connection in ruby using mysql adapter"
date: 2009-11-12T20:27:00+05:30
comments: false
categories:
 - ruby
 - mysql
 - active record
---
## Manual DB connection
Below ruby code establishes a manual database connection.
```ruby
require 'active_record'
ActiveRecord::Base.establish_connection(
  :adapter  => "mysql",     
  :host     => "localhost",     
  :username => "root",     
  :password => "abcd",     
  :database => "funonrails"   
) 
```

## Snippet of database.yml file
```yml
common: &common
  adapter: mysql   
  database: students_dev   
  username: root   
  password:   
  host: localhost  

students_development:   
  <<: *common   
  database: students_dev  

students_production:   
  <<: *common   
  database: students 
```

## Load database configurations from yml file
```ruby
dbconfig = YAML::load(File.open('database.yml'))  
ActiveRecord::Base.establish_connection( dbconfig[:students_development] ) 
```

## Active Record Model & DB connection 
```ruby
class Student < ActiveRecord::Base
  establish_connection "students_production"   
  set_table_name "student"   
  set_primary_key "stud_number" 
end 
```
