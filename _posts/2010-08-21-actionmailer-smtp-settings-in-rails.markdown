---
layout: post
title: "ActionMailer SMTP settings in rails"
date: 2010-08-21T03:48:00+05:30
comments: false
categories:
 - rails
---

**1. Add following line to rails environment file**
```ruby
ActionMailer::Base.delivery_method = :smtp
```
**2. Include your email authentication**

Create a ruby file called `smtp_settings` under `config/initializers` directory
```ruby
# config/initializers/smtp_settings.rb
ActionMailer::Base.smtp_settings = {
  :address => "smtp.gmail.com",
  :port => 587,
  :authentication => :plain,
  :enable_starttls_auto => true,
  :user_name => "replies@gmail.com",
  :password => "_my_gmail_password_"
}
```

**note:** Keep `starttls_auto` always `true`

**3. Create sample mailer in order to ensure that your settings are correct**

Create a sample mailer
```
class EmailMailer < ActionMailer::Base
 def test_email
  from 'xx@gmail.com'
  to 'some@xx.com'
  subject "This is test email"
  message "It should get delivered to recipient inbox"
 end
end
```
**4. Test your configuration**
```ruby
EmailMailer.deliver_test_email
```
