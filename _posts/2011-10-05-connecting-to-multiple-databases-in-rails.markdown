---
layout: post
title: "Legacy database connections in rails"
date: 2011-10-05T13:13:00+05:30
comments: false
categories:
 - rails
 - active record
---

> While developing rails application we often come across situation where you wanted different rails models connecting different tables of different databases.
 
Example: We are having ndnc model holding ndnc records associated with database ndnc. 
Lets create ConnectionBase model that will connect to other databse 'ndnc' and that will be extended from ActiveRecordBase and put that inside library files 

```ruby
# lib/connection_base.rb 

class ConnectionBase < ActiveRecord::Base

  establish_connection(
    :adapter => "postgresql",
    :username=> "sandip",
    :password => "2121",
    :database => "ndnc"
)
end
```

Wherever you wanted to connect to other database i.e. ndnc you can extend that particular model with ConnectionBase 

```ruby
class Ndnc < ConnectionBase
# connects to ndnc table of ndnc database
end
```

Please make sure library files are loaded while server startup
```ruby
# vi config/application.rb
config.autoload_paths += %W(#{config.root}/lib)
```
Done !
In cases where you want only one particular model connecting other database table, you can put connection setting itself in that model.
