---
layout: post
title: "Testing restful & polymorphic resource routes on rails console"
date: 2011-01-19T15:05:00+05:30
comments: false
categories:
 - rails
 - routes
---

Testing restful & polymorphic resource routes on rails console 

## Open rails console
```ruby
rails c # In rails 3
# OR
ruby script/console
```
## Getting app object
```
>> app

=> #<ActionController::Integration::Session:0xc164fac @result=nil, @status=nil, @accept="text/xml,application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5", @path=nil, @request_count=0, @application=#<ActionController::Dispatcher:0xc1644f8 @output=#<IO:0x85e34ec>>, @remote_addr="127.0.0.1", @host="www.example.com", @controller=nil, @https=false, @request=nil, @headers=nil, @cookies={}, @status_message=nil, @named_routes_configured=true, @response=nil>
```

## Root URL
```
>> app.root_url
=> "http://www.example.com/"
```

## Plural paths
```
>> app.calls_path
=> "/calls"

```

## Singular routes
```
>> app.audio_call_path
ActionController::RoutingError: audio_call_url failed  


>> app.audio_call_path(1)
=> "/calls/1/audio"


>> app.audio_call_path(13)
=> "/calls/13/audio"


>> app.audio_call_path(13, :name => 'san')
=> "/calls/13/audio?name=san"
```

## Polymorphic URLS
```
app.polymorphic_path(Call.first, :action => 'audio')
=> "/calls/253/audio"



>> app.polymorphic_path(Call.new)
=> "/calls"

```
