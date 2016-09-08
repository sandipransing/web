---
layout: post
title: "using thinking sphinx search in rails app"
date: 2010-04-12T19:07:00+05:30
comments: false
categories:
 - rails
 - thinking sphinx
---

I am using *acts_as_ferret* and *ferret server* as of now with my rails application and it just works fine for me. The ONLY problem is **performance** as it takes a lot time to build index and to rebuild index when it gets screwed up and that's where **sphinx rocks**! 

To get started with thinking sphinx you need to install sphinx server first. for installation help click here. 

To use sphinx search in rails we need to use either thinking sphinx gem or plugin that can be easily find on github. 

## Plugin installation 
```
ruby script/plugin install  git://github.com/freelancing-god/thinking-sphinx.git
```

#### OR
## Gem installation

To install gem run the following command. 
```
gem install thinking-sphinx --source http://rubygems.org/

If you're upgrading, you should read this:
http://freelancing-god.github.com/ts/en/upgrading.html

Successfully installed thinking-sphinx-1.3.16
1 gem installed
```

Edit environment file and add following lines to it inside initializer block. 
```
config.gem(
     'thinking-sphinx',
      :lib     => 'thinking_sphinx',
      :version => '1.3.16'
 )
```
Edit Rakefile and add following lines to it.

```
require 'thinking_sphinx/tasks'
```

List rake tasks that should show up sphinx related tasks. 
```
rake -T ts
(in /home/sandip/v1)
rake doc:plugins:acts_as_audited  # Generate documentation for the acts_as_...
rake doc:plugins:acts_as_ferret   # Generate documentation for the acts_as_...
rake rails:update:javascripts     # Update your javascripts from your curre...
rake rails:update:scripts         # Add new scripts to the application scri...
rake stats                        # Report code statistics (KLOCs, etc) fro...
rake test:units                   # Run tests for unitsdb:test:prepare / Ru...
rake tmp:sockets:clear            # Clears all files in tmp/sockets
rake ts:conf                      # Generate the Sphinx configuration file ...
rake ts:config                    # Generate the Sphinx configuration file ...
rake ts:in                        # Index data for Sphinx using Thinking Sp...
rake ts:rebuild                   # Stop Sphinx (if it's running), rebuild ...
rake ts:reindex                   # Reindex Sphinx without regenerating the...
rake ts:restart                   # Restart Sphinx / Restart Sphinx
rake ts:run                       # Stop if running, then start a Sphinx se...
rake ts:start                     # Start a Sphinx searchd daemon using Thi...
rake ts:stop                      # Stop Sphinx using Thinking Sphinx's set...
rake ts:version                   # Output the current Thinking Sphinx vers...
```

## Starting Sphinx

Start sphinx server this should give up an error.
```
rake ts:start
(in /home/sandip/v1)
Failed to start searchd daemon. Check /home/sandip/v1/log/searchd.log.
```

## Adding indexes in models
```ruby
define_index do
     # following fields are database fields
     # we can not add model methods in sphinx index 
     # sphinx fields allows ONLY model based associations
     # fields
      indexes customer_name
      indexes phone
      indexes mobile
      indexes other_phone
      indexes car_make
      indexes car_model
      indexes registration_no
end
```
## Configure sphinx 
```
rake ts:config
Generating Configuration to /home/sandip/v1/config/development.sphinx.conf
Index sphinx

rake ts:in
(in /home/sandip/v1)
Generating Configuration to /home/sandip/v1/config/development.sphinx.conf
Sphinx 0.9.8-rc2 (r1234)
Copyright (c) 2001-2008, Andrew Aksyonoff

using config file '/home/sandip/v1/config/development.sphinx.conf'...
indexing index 'call_core'...
collected 113521 docs, 2.1 MB
collected 0 attr values
sorted 0.1 Mvalues, 100.0% done
sorted 0.3 Mhits, 100.0% done
total 113521 docs, 2114790 bytes
total 6.102 sec, 346589.68 bytes/sec, 18604.78 docs/sec
distributed index 'call' can not be directly indexed; skipping.
Generating Configuration to /home/sandip/v1/config/development.sphinx.conf
Sphinx 0.9.8-rc2 (r1234)
Copyright (c) 2001-2008, Andrew Aksyonoff

using config file '/home/sandip/v1/config/development.sphinx.conf'...
indexing index 'call_core'...
collected 113521 docs, 2.1 MB
collected 0 attr values
sorted 0.1 Mvalues, 100.0% done
sorted 0.3 Mhits, 100.0% done
total 113521 docs, 2114790 bytes
total 3.628 sec, 582909.70 bytes/sec, 31290.34 docs/sec
distributed index 'call' can not be directly indexed; skipping.
```
## Search using sphinx
```ruby
ruby script console

>>Call.search 'sandip'
=> [#<call 00:00:00"="" 09:22:35",="" 1018,="" 18:10:00",="" 2009-02-10="" 2009-03-27="" 2009-04-07="" 2009-11-18="" 20:29:06",="" 6,="" created_at:="" id:="" insurance_expiry_date:="" is="" is_existing_customer:="" nil,="" note:="" phone="" ringing",="" scheduled_at:="" state_id:="" station_shift_customer_car_insurance_product_id:="" updated_at:="">, #<call 00:00:00"="" 11:13:33",="" 18:30:00",="" 2009-02-10="" 2009-02-16="" 2009-04-03="" 2009-11-18="" 20:34:52",="" 6,="" 6024,="" call="" created",="" created_at:="" customer="" id:="" insurance_expiry_date:="" is_existing_customer:="" new="" nil,="" note:="" scheduled_at:="" state_id:="" station_shift_customer_car_insurance_product_id:="" updated_at:="">] </call></call>
```
It returns array of records found. conditions are allowed by sphinx. If you need to add conditions on integer attributes then index block in model needs to have has method like author_id in following. 
```ruby
define_index do
  indexes content
  indexes :name, :sortable => true
  indexes comments.content, :as => :comment_content
  indexes [author.first_name, author.last_name], :as => :author_name
  
  has author_id, created_at
end
```
