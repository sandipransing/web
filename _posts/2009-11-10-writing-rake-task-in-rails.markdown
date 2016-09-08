---
layout: post
title: "Writing rake task in rails with namespace, parameters"
date: 2009-11-10T15:09:00+05:30
comments: false
categories:
 - rails
 - rake-tasks
---

Rake tasks itself defines that "they are bunch of ruby code that performs some task."

Rake tasks are placed in lib/tasks directory of application and files have `.rake` extension.
There are many lovable tasks defined in rails. They can be executed from console.

## Benefits of writing rake task :
1.  Testing of code 
2.  Scheduled rake tasks ( backgroundRb and scheduled tasks using cron ) 
3.  Simplifies code.

Lets, understand what is code inside rake tasks.

## Simple greet rake task
``` 
# lib/tasks/welcome.rake
task :greet do
  puts "Hello !!"
end
```

## Execute task
```
rake greet
```

## Adding description to rake task 
```ruby
# lib/tasks/welcome.rake
desc "This is new style of greet"
task :greet do
  puts "Hello !!"
end
```

## Execute task
```
rake greet
```

## Adding namespace to rake tasks

It’s nothing but prefix that takes while executing rake task. Benefit of adding namespace is to categories similar rake tasks. 
```
# lib/tasks/welcome.rake
namespace :introduction do
  desc "This is one style of introduction"
  task :greet do
    puts "Hello !!"
  end

  desc "This is 2nd style of introduction"
  task :hi do
    puts "Hi "
  end
end
```
## Execute task
```
rake introduction:greet
rake introduction:hi
```
## Passing arguments to rake tasks 

```ruby
# lib/tasks/welcome.rake
namespace :introduction do
  desc "This is one style of introduction"
  task :greet do
    puts "Hello !!"
  end
  
  # Here rails environment is loaded
  # Here you can access model, controllers, etc
  desc "This is 2nd style of introduction"
  task :hi => :enviroment do
    puts "Hi #{ENV['name']}"
  end
end
```
## Execute task
```
rake introduction:hi name=Raj
```
You can read more on this [here](http://www.blogger.com/For%20more%20reference%20http://railsenvy.com/2007/6/11/ruby-on-rails-rake-tutorial#namespaces)
