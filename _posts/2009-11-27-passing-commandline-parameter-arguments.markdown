---
layout: post
title: "Passing commandline parameter (arguments) to ruby file using optparser"
date: 2009-11-27T19:13:00+05:30
comments: false
categories:
 - ruby
 - optparser
---

Ruby file accepts from command prompt in the form of array
## Passing parameters 
```
ruby input.rb TOI DH TimesNew
```
## Accessing parameters 
```ruby
# input.rb
p ARGV
# => ["TOI", "DH", "TimesNew"]
p ARGV[0]
# => "TOI"
p ARGV[1]
# => "DH"
```
Optparser : parses commandline options in more effective way using OptParser class. and we can access options as hashed parameters.

##  Passing parameters 
```
ruby input.rb -P"The Times of India" --category"article"
```
##  Accessing parameters 
```ruby
# vi input.rb
require 'optparser'
 
options = ARGV.getopts("P:", 'category')

p options
# => { 'P' => 'The Times of India', :category => 'article' }

p options['P']
# => "The Times of India"
```
