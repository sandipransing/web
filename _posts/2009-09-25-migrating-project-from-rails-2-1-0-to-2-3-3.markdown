---
layout: post
title: "Migrating project from rails 2.1.0 to edge rails 2.3.3"
date: 2009-09-25T01:25:00+05:30
comments: false
categories:
 - rails
---

I wanted to migrate my project from rails `2.1.0` to rails `2.3.3`

so, i performed following steps, but it shows me memcached-client error.

```
# vi config/environment.rb

# Specifies gem version of Rails to use when vendor/rails is not present
RAILS_GEM_VERSION = '2.3.3' unless defined? RAILS_GEM_VERSION

ruby script/server
```

It shows me error
```
`install_memcached': 'memcache-client' client requested but not installed. Try 'sudo gem install memcache-client'. (Interlock::ConfigurationError)
```
so i installed `memcache-client` gem
```
gem install memcache-client
```
but still i am showing same error, any ideas ??

## Comments

**JimmyBean**

I don't know If I said it already but …I'm so glad I found this site…Keep up the good work I read a lot of blogs on a daily basis and for the most part, people lack substance but, I just wanted to make a quick comment to say GREAT blog. Thanks, :)

A definite great read..Jim Bean
