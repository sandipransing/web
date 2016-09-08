---
layout: post
title: "paperclip options aws-s3 config and rails"
date: 2011-11-21T23:23:00+05:30
comments: false
categories:
 - rails
 - paperclip
 - aws
---

Stepwise guide to configure paperclip default options, setting up aws-s3 storage in rails 
Inside Gemfile

```
gem 'aws-s3',               :require => 'aws/s3'
gem 'paperclip'
```

Bundle
```
bundle install
```
Generate Print model to hold image 
```
rails g model print image_file_name:string image_content_type:string image_file_size:string
```

DB Migration
```
rake db:migrate
```

Add s3 credentials to YML file
```
# config/s3.yml 
access_key_id: DASDFG7KACNxIJdJXHPQ
secret_access_key: BnDrTnzCTX+R707wYEP/aCEqAsDFG7sgW
```

Add default paperclip attachment options to initializer
```ruby
# Make sure to add host url inside config/environments
# HOSTNAME = 'http://lordganesha.com'

Paperclip::Attachment.default_options.merge!(
  :storage => 's3',
  :s3_credentials => YAML.load_file("#{Rails.root}/config/s3.yml"),
  :path => ":class/:attachment/#{Rails.env}/:id/:style/:basename.:extension",
    :default_url => "http://#{HOSTNAME}/images/paperclip/:class/:attachment/default.jpg",
  :bucket => 'ganesha'
)
```

Add image attachment code to print model
```ruby
# app/models/print.rb
class Print < ActiveRecord::Base
  has_attached_file :image,
    :styles => {:medium => ["400x400#", :jpg], 
                :thumb => ["100x100#", :jpg], 
                :slider => ["300x300#", :jpg]}
  #validates_attachment_presence :image
  validates_attachment_size :image, :less_than => 1.megabytes, :message => 'file size maximum 1 MB allowed'
  validates_attachment_content_type :image, :content_type => ['image/jpeg', 'image/png', 'image/gif', 'image/bmp', 'image/pjpeg', 'image/x-png']
end
```

Inside views
```haml
# inside new.html.haml 
= form_for @print do
  =f.file_field :image
```

