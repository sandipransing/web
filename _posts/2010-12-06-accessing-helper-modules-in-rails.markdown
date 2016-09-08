---
layout: post
title: "Accessing Helper modules in rails"
date: 2010-12-06T15:51:00+05:30
comments: false
categories:
 - rails
 - views
---

Methods defined in Helper modules can directly accessed in rails views because this is what they are pretended for but we often come across with situations where we wanted to use some helper methods in controllers, views, models and mailers and obvious we don't want to repeat same lines of code everywhere which also rail does not permit. forgotten DRY? Oh then how to do achieve same without violating rails principle.

Certainly there are ways to do this ..

## 1. Helper methods all the time for views
```ruby
class ApplicationController < ActionController::Base
  helper :all# include all helpers, all the time for views
end
```
## 2. Controller methods in views
```ruby
class ApplicationController < ActionController::Base

  helper_method :current_store
  #now controller_method can be accessed in views
  ...
end
```

## 3. Helper methods in controller
```ruby
class ApplicationController < ActionController::Base

  include ActionView::Helpers::ApplicationHelper
  ...

end
```

## 4. Helper methods in model
```ruby
class Student < ActiveRecord::Base

  include ActionView::Helpers::ApplicationHelper
  ...

end
```

## 5. Helper methods in mailers
```ruby
class Notifier < ActionMailer::Base

  add_template_helper(ApplicationHelper)
  ...

end
```
