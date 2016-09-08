---
layout: post
title: "authlogic custom conditions for authentication"
date: 2010-11-28T23:23:00+05:30
comments: false
categories:
 - rails
 - authlogic
---

To Add custom conditions to authlogic finders first of all we need to override authlogic find_by_login method. 

```ruby
class UserSession < Authlogic::Session::Base

  after_validation :check_if_verified

  find_by_login_method :find_by_login_and_deleted_method

end
```

Then we need to define overridden method inside User model 

```ruby
class User < ActiveRecord::Base

  acts_as_authentic

  def self.find_by_login_and_deleted_method(login)

    find_by_email_and_deleted(login, false)

  end

end
```
got easy..wooooooo :)
