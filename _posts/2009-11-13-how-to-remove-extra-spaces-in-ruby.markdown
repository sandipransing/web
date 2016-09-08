---
layout: post
title: "How to remove extra spaces in ruby strings"
date: 2009-11-13T14:15:00+05:30
comments: false
categories:
 - ruby
---

There are many methods in ruby which will allow to remove blank spaces from ruby strings (sentenses)

#### squeeze
This will remove extra spaces (more than one) in between two words

```ruby
"Write      your       string            here   ".squeeze.strip
```

#### gsub
It can be used to match and substitue additional spaces
```ruby
"This     is my      input  string     ".gsub(/ +/, ' ')
```

#### split & join strings
Strings can be splitted based on spaces and joined.
```ruby
"Write      your       string          here   ".split('  ').join(' ')
```
