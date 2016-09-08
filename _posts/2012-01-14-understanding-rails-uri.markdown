---
layout: post
title: "understanding rails uri"
date: 2012-01-14T02:18:00+05:30
comments: false
categories:
 - rails
 - rails-uri
---
`rails-uri` module provides us with url manipulation methods

## Parse string url
```ruby
url = URI.parse('http://funonrails.com/categories/rails-3')
url.host #=> "http://funonrails.com"
url.port #=> 80
```

## URL with Basic Authentication 
```ruby
url = URI.parse('http://sandip:2121@funonrails.com/categories/rails-3')
url.user #=> "sandip"
url.password #=> "2121"
```

## Extracting urls form string paragraph
```ruby
URI.extract('http://funonrails.com is rails blog authored by http://sandipransing.github.com contact mailto://sandip@funonrails.com')
#=> ["http://funonrails.com", "http://sandipransing.github.com", "mailto://sandip@funonrails.com"]
```

## Split & Join URI
```ruby
URI.split('http://sandip:2121@funonrails.com/categories/rails-3')
#=> ["http", "sandip:2121", "funonrails.com", nil, nil, "/categories/rails-3", nil, nil, nil]

<=> [Scheme, Userinfo, Host, Port, Registry, Path, Opaque, Query, Fragment]
  
URI.join('http://funonrails.com','categories/rails-3')
#=> #<URI::HTTP:0x7fbf9202efc8 URL:http://funonrails.com/categories/rails-3>
```
## Escape & Unescape alias encode/decode URI
```ruby
URI.escape "http://funonrails.com/categories/named scopes"
#=> "http://funonrails.com/categories/named%20scopes"

URI.encode "http://funonrails.com/categories/named scopes"
#=> "http://funonrails.com/categories/named%20scopes"

URI.decode "http://funonrails.com/categories/named%20scopes"
#=> "http://funonrails.com/categories/named scopes"

URI.unescape "http://funonrails.com/categories/named%20scopes"
#=> "http://funonrails.com/categories/named scopes"
```

## Match urls using regular expressions 

```
# extract first URI from html_string
"ftp://localhost http://funonrails.com/categories/rails-3".slice(URI.regexp)
#=> "ftp://localhost"

# remove ftp URIs
"ftp://localhost http://funonrails.com/categories/rails-3".sub(URI.regexp(['ftp']))
#=> " http://funonrails.com/categories/rails-3"

# You should not rely on the number of parentheses
"ftp://localhost http://funonrails.com/categories/rails-3".scan(URI.regexp(['ftp'])) do |*matchs|
  p $&
  #=> ftp://localhost
end
```

## Getting requested url inside rails
```ruby
request.request_uri
request.env['REQUEST_URI']
```

## Getting previous page url inside rails
```
request.referrer
```
