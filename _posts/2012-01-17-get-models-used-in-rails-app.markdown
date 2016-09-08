---
layout: post
title: "Get models list inside rails app"
date: 2012-01-17T05:16:00+05:30
comments: false
categories:
 - active record
 - rails 3
---
How to get collection of models inside your application. Certainly there are many ways to do it.
Lets have a look at different ways starting from worst - 

Get table names inside database and then iterating over to get model name
```ruby
@models = ActiveRecord::Base.connection.tables.collect{|t| t.underscore.singularize.camelize}

#=> ["AdhearsionAudit", "AudioLog", "AuditDetail","TinyPrint", "TinyVideo", "UnknownCall", "UserAudit", "User"]
```

Select those with associated class
```ruby
@models.delete_if{|m| m.constantize rescue true}
```

Load models dir
```ruby
@models = Dir['app/models/*.rb'].map {|f| File.basename(f, '.*').camelize.constantize.name }
```

Select ActiveRecord::Base extended class only
```ruby
@models.reject!{|m| m.constantize.superclass != ActiveRecord::Base }
```

Get Active Record subclasses
```ruby
# make sure relevant models are loaded otherwise
# require them prior
# Dir.glob(RAILS_ROOT + '/app/models/*.rb').each { |file| require file }
class A < ActiveRecord::Base
end
class B < A
end
ActiveRecord::Base.send(:subclasses).collect(&:name)
#=> [...., A]
```

How to get Inherited models too 
```ruby
class A < ActiveRecord::Base
end
class B < A
end
ActiveRecord::Base.descendants.collect(&:name)
#=> [...., A, B]
```

Below is more elegant solution provide by [Vincent-robert](http://stackoverflow.com/users/268/vincent-robert) over stack overflow which recursively looks for subsequent descendent's of class and gives you list from all over application
```ruby
class Class
  def extend?(klass)
    not superclass.nil? and ( superclass == klass or superclass.extend? klass )
  end
end

def models 
  Module.constants.select do |constant_name|
    constant = eval constant_name
    if not constant.nil? and constant.is_a? Class and constant.extend? ActiveRecord::Base
    constant
    end
  end
end
```
