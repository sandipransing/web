---
layout: post
title: "Email attachments in ruby & rails"
date: 2010-08-21T03:30:00+05:30
comments: false
categories:
 - action-mailer
 - rails
---

**1. Add ActionMailer configuration in environment**

This configuration can be different for development and production.
```ruby
# Include your application configuration below
# You can set two configurations sendmail as well as smtp
# To use SMTP you need to provide your email account credentials
# Sendmail is a unix package that needs to be installed and configured while 
# using sendmail settings
# Chances of getting emails into recipient's inbox are 100% for smtp settings
# whereas sendmail needs some other configurations to be done before using.

ActionMailer::Base.delivery_method = :smtp 
ActionMailer::Base.default_content_type = "text/html"
```

**2. Create Mailer Model and add method to deliver email**

```ruby
class EmailMailer < ActionMailer::Base

  def email_with_attachments(email, files=[])  
    # content type also can be set in environment file as
    # ActionMailer::Base.default_content_type = "text/html"
    @headers = {content_type => 'text/html'} 
    @sent_on = Time.now  
    @recipients = email.recipients
    @from = email.from
    @cc = FEEDBACK_RECIPIENT
    @subject = email.subject
    @body = email.message
    
    # attach files  
    files.each do |file|  
      attachment "application/octet-stream" do |a|  
        a.body = file.read  
        a.filename = file.original_filename  
      end unless file.blank?  
    end  

end
```

**3. Email Model**

```ruby
# This is the virtual model in rails which has no database table associated with it
class Email < ActiveRecord::Base
  # It uses has_no_table plugin to create virtual model
  # This can also be done using following lines of code
  # 
  # def self.columns() @columns ||= []; end
  # def self.column(name, sql_type = nil, default = nil, null = true)
  #  columns << ActiveRecord::ConnectionAdapters::Column.new(name.to_s, default, sql_type.to_s, null)
  # end
  #

  has_no_table
  #insert the names of the form fields here
  column :from, :string
  column :recipients, :string
  column :subject, :string
  column :message, :text
  column :call_id, :integer

  attr_accessor :is_subscribed

  #Validations goes here
  validates_presence_of :from, :message => "You dont have Email ID, you cannot continue!", :unless => "call_id.blank?"
  validates_format_of :from, :with => /^([^@]+)@((?:[-a-z0-9]+.)+[a-z]{2,})$/i, :unless => "from.blank?"
  validates_presence_of :recipients
  validates_format_of :recipients, :with => /^([^@]+)@((?:[-a-z0-9]+.)+[a-z]{2,})$/i, :message => "Invalid email format", :unless => "recipients.blank?"
  validates_presence_of :subject, :message
end
```

**4. Usage from console or controller**

```
# Here email is the valid object of email model
# attachments is the area of files to be attached with email
# In my case attachments are of kind of pdf files
# you can specify type of attachment in your mailer method
EmailMailer.deliver_email_with_attachements(email, attachments) if email.valid?
```

#### Sending data stream as email attachments in rails

In some cases, We need to dynamically generate files and you don't want to store them locally on file system instead you always like to email them from memory itself

Here is the way to do that:

**1. Mailer data stream as attachment method**

```ruby
class EmailMailer < ActionMailer::Base

  def email_with_data_stream(email, data_stream=[])  
    @headers = {}  
    @sent_on = Time.now  
    @recipients = email.recipients
    @from = email.from
    @subject = email.subject
    @body = email.message

    # attach files  
    data_stream.each do |data|  

      attachment "application/octet-stream" do |a|  
        a.body = data[0]
        a.filename = data[1] 
      end unless data_stream.blank?  
    end  
  end  
end
```

**2. Lets take example of pdf renderer**
```ruby
# Assume we have pdf object
attachments = []
data = pdf.render
# Attach as many files you wanted. Be careful about email maximum size ;)
attachments << data
EmailMailer.deliver_email_with_data_stream(email, attachments)
```
