---
layout: post
title: "Hpricot scraping in ruby"
date: 2009-12-02T18:24:00+05:30
comments: false
categories:
 - ruby
 - api
 - hpricot
---

Include gems/library required before getting started 
```ruby
require 'hpricot'
require 'net/http'
require 'rio'
# Pass website url to be scraped
url = "www.funonrails.com"

# Define filename to store file locally
file = "temp.html"
# Save page locally
rio(url) < rio (file)
# Open page through hpricot
doc = Hpricot(open(file))
```
Apply hpricot library to get right contents
```ruby
doc.at("div.pageTitle")
doc/"div.pageTitle"
doc.search("div.entry")
doc//"div.pageTitle"
```
Hpricot API Reference click [here](http://rdoc.info/github/hpricot/hpricot/Hpricot/Doc)
