---
layout: post
title: "Getting assets pipeline working on heroku"
date: 2011-12-22T03:14:00+05:30
comments: false
categories:
 - rails 3
 - heroku
---

> Heroku is readonly file system hence write operations can only be done inside tmp directory. Asset compilation on heroku can be handled in different ways.

**1. compiling locally and placing inside assets directory**
```
RAILS_ENV=production bundle exec rake assets:precompile
```

**2. It compiles assets while slug compiles**

**3. Run time asset compilation**
It compiles assets for every rails request if it notifies assets got modified

Configurations
```ruby
config/application.rb
# Enable the asset pipeline
config.assets.enabled = true

# Version of your assets, change this if you want to expire all your assets
config.assets.version = '1.0'
config.assets.prefix = Rails.root.join('tmp/assets').to_s

# config/environments/production.rb
# Don't fallback to assets pipeline if a precompiled asset is missed
config.assets.compile = true
```

Note: write operation made inside tmp directory can not make any guarantee of being persisted hence be careful while using.
