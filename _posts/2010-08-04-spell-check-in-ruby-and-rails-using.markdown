---
layout: post
title: "Spell Check in ruby and rails using BOSSMan"
date: 2010-08-04T02:26:00+05:30
comments: false
categories:
 - ruby
 - rails
 - bossman
---

Wrong English is an often problem while developing any website product as it gives bad view to website user and thus does direct impact on product. **BOSSMan** is a ruby gem that interacts with **yahoo web service** and provides a simplest way to overcome such errors.

#### Installation:
```
gem sources -a http://gems.github.com
gem install jpignata-bossman
```
Apply and get Application ID from yahoo developer network

`URl https://developer.apps.yahoo.com/`

Make sure to note it for reference

Usage in ruby app
```ruby
require 'rubygems'
require 'bossman'
include BOSSMan

BOSSMan.application_id = "Your Application ID here"
```

#### Spelling Suggestions

```ruby
text = BOSSMan::Search.spelling("gooogle")
=> #{"resultset_spell"=>[{"suggestion"=>"google"}], "responsecode"=>"200", "deephits"=>"1", "start"=>"0", "count"=>"1", "totalhits"=>"1"}}>

text.suggestion 
=> "google"
```

More sophisticated way of use -

1. Create a YML file containing list of kewords 
2. Load YML file 
3. Iterate YML hash to find out spell suggestions
**Example: spelling.yml**
```
  1 keywords:
  2   gooogle:
  3   Barack Oabama:
  4   Indian:
  5   Latuur:
```
```ruby
keywords = YAML.load_file('spelling.yml')['keywords'].keys

  puts "Correction suggested"
  keywords.each do |keyword|
    text = BOSSMan::Search.spelling(keyword)
    if defined? text.suggestion
      puts "#{keyword} => #{text.suggestion}"
    end
  end
```
#### Output
```
Correction suggested
gooogle => google
Barack Oabama => Barack Obama
Latuur => Latour
```

Analyze suggestions manually and make neccesary corrections..
