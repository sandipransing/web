---
layout: post
title: "YAML file configuration in ruby & rails"
date: 2009-11-17T15:01:00+05:30
comments: false
categories:
 - rails
 - YAML
---
While coding in ruby and rails, we often requires variables to be initialized that can be used across application. There are many ways to define configuration variables 

## Initialize variables inside environment
```
# This agencies can be used across application
AGENCIES = ['TIMES NEWS NETWORK', 'AGENCIES', 'AFP', 'PTI']

# Configuration for html nodes

Article_title_tag = "h1.heading"
Date_auth_agency_tag = "span[@class='byline']"
Location_content_tag = "div[@class='Normal']"
```
## Create config inside initializers

Just make `config.rb` inside initializers and add variables to it. 
```
#config/initializers/config.rb
# This agencies can be used across application
AGENCIES = ['TIMES NEWS NETWORK', 'AGENCIES', 'AFP', 'PTI']

# Configuration for html nodes
Article_title_tag = "h1.heading"
Date_auth_agency_tag = "span[@class='byline']"
Location_content_tag = "div[@class='Normal']"
```

## Load config using YML

But most efficient way to organize variables is YML file
```yml
# config/config.yml
the_times_of_india:
  article_title_tag: h1.heading
  date_auth_agency_tag: span[@class='byline']
  loc_content_tag: div[@class='Normal']
```
## Accessing YML files 

```ruby
APP_CONFIG = YAML.load_file("#{RAILS_ROOT}/config/config.yml")
#=> true
>> APP_CONFIG
#=> {"the_times_of_india"=>{"article_title_tag"=>"h1.heading", "loc_content_tag"=>"div[@class='Normal']", "date_auth_agency_tag"=>"span[@class='byline']"}}
>> APP_CONFIG[:the_times_of_india]
#=> nil
>> APP_CONFIG['the_times_of_india']
#=> {"article_title_tag"=>"h1.heading", "loc_content_tag"=>"div[@class='Normal']", "date_auth_agency_tag"=>"span[@class='byline']"}
>> APP_CONFIG['the_times_of_india']['article_title_tag']
#=> "h1.heading"
```
