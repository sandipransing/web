---
layout: post
title: "--no-ri --no-rdoc for ruby gem installation"
date: 2009-09-30T23:27:00+05:30
comments: false
categories:
 - gemrc
 - --no-ri --no-rdoc
---

## GEM install without rdoc 
```
gem install GEM_NAME –no-ri –no-rdoc
```
Skip ri rdoc for all gem installation as i haven’t seen anyone using it.

## Edit gemrc file
Add following line to it
```
# vi ~/.gemrc
:gem: --no-ri --no-rdoc
```

Here is my `~/.gemrc` file
```
---
:verbose: true
gem: --no-ri --no-rdoc
:update_sources: true
:sources:
- http://gems.rubyforge.org
- http://gems.github.com
:backtrace: false
:bulk_threshold: 1000
:benchmark: false
```

That’s it !

## Comments

**@Tnelson**
I don’t usually reply to posts but I will in this case, great info…I will add a backlink and bookmark your site. Keep up the good work!
