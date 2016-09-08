---
layout: post
title: Upgrading from Rails 2.1.x to Rails 2.3.11
date: 2012-04-04T22:13:00+05:30
comments: false
categories:
 - rails
---
If your application is currently on any version of Rails 2.1.x,
Then following changes needs to be done for upgrading your application to Rails `2.3.11`

**1. First install Rails version `2.3.11`**
```
gem install rails -v2.3.11
```

**2. Freeze app ruby gems**
```
rake rails:freeze:gems
```
Hopefully it should work for you but it gave me following error
> undefined method `manage_gems' for Gem:Module

**3. Create sample rails 2.3.11 app**
```
rails _2.3.11_ testsapp
```
<!--more-->
Now, Copy all missing "config/initializers/*" files from newly created "testapp" to the application that is to be upgraded.
```
cp testapp/config/initializers/* config/initializers
```

**4. Change Rails version inside `environment.rb` to `Rails 2.3.11`**
```ruby
# Specifies gem version of Rails to use when vendor/rails is not present
RAILS_GEM_VERSION = '2.3.11'
```

**5. Rename <i>app/controllers/application.rb</i> file to <i>app/controllers/application_controller.rb</i>**

**OR**
```
rails:update:application_controller
```

**6. Start rails server and fix the issues one by one**
```
ruby script/server
```
