---
layout: post
title: "Accessing View Helpers & Routes in Rails Console, Rake Tasks and Mailers"
date: 2011-03-17T01:01:00+05:30
comments: false
categories:
 - rails
---

While looking for accessing rails template tags to be accessed on rails console in order to examine whatever written needs to be correct, found that - rails template tags can be tested on rails console using *helper* object

```ruby
module ApplicationHelper
  def display_amount(amount)
    number_to_currency amount, :precision => 0
  end  
end
```

## Helpers on rails console
```ruby
rails c
>> helper.text_field_tag :name, 'sandip'
=> "<input id="name" name="name" type="text" value="sandip" />"
>> helper.link_to 'my blog', 'http://funonrails.com'
=> "<a href=''http://funonrails.com>my blog</a>"
>> helper.display_amount(2000)
=> "$2,000"
```

isn't it cool ? though it's not finished yet. we can include helper files on console and access custom helper methods too 
```ruby
>> include ActionView::Helpers
=> Object
>> include ApplicationHelper
=> Object
>> include ActionView::Helpers::ApplicationHelper^C
>> display_amount 2500
=> "$2,500"
```

Using same way one can make use helpers in rake tasks file. Make sure environment is getting loaded into rake task.

```ruby
 lib/tasks/helper_task.rake
namespace :helper do
 desc 'Access helper methods in rake tasks'
 task :test => :environment do
  include ActionView::Helpers
  include ApplicationHelper

  puts display_amount 2500  #=> "$2,500"
 end
end
```

## Accessing helpers in Action Mailers
```ruby
class Notifier < ActionMailer::Base
  add_template_helper(ApplicationHelper)
  def greet(user)
    subject "Hello #{user.name}"
    from          'no-reply@example.com'
    recipients    user.email
    sent_on       Time.now
    @user = user
  end
```

```html
# app/views/notifier/greet.html.erb
Hi <%= @user.try(:name)%>, 



Greetings !!!
You have got <%= display_amount(3000) %>
```

## Rails routes on rails console
```ruby
rails c
>> app.root_url
=> "http://www.example.com/"
>> app.change_password_path
=> "/change/password"
>> app.change_password_url
=> "http://www.example.com/change/password"
```

Now you may have got requirement to access routes inside rake tasks. Here is the way to do that - 
```ruby
# lib/tasks/route_test.rake
namespace :routes do
 desc 'Access helper methods in rake tasks'
 task :test => :environment do
  include ActionController::UrlWriter
  default_url_options[:host] = "myroutes.check"
  puts root_url  #=> "http://myroutes.check"
 end
end
```

Optionally you can set value of host parameter from action mailer host configuration using

```ruby
default_url_options[:host] = ActionMailer::Base.default_url_options[:host]
```
Attention: This is my preferred way to use helpers and routes when needed and there may have other choices to do same. Anyone having better approach please feel free to add your valuable comment. I will definitely incorporate that in further developments.
Got Easy, Cheers ;)
