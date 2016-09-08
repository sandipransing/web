---
layout: post
title: "Using hoptoad notifier as gem in rails 2.3"
date: 2011-01-27T21:18:00+05:30
comments: false
categories:
 - hotoad
 - rails
---

**1.  Add GEM to environment file**
```
# in config/environment.rb
config.gem 'hoptoad_notifier'
```

**2. Install gems**
```
# from your Rails root:
rake gems:install
rake gems:unpack GEM=hoptoad_notifier
```

**3. Generate hoptoad install using key**
```
ruby script/generate hoptoad --api-key your_key_here
```
