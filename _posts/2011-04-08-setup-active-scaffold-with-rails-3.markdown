---
layout: post
title: "Setup Active Scaffold with rails 3 using jQuery"
date: 2011-04-08T18:27:00+05:30
comments: false
categories:
 - rails
 - active-scaffold
 - jQuery
---

Create new rails project
```
rails new demo -d mysql
```

Add active_scaffold gem to bundler
```
gem "active_scaffold_vho"
```

Run bundle install command 
```
bundle install
```

Setup active scaffold to use jQuery 
```
rails g active_scaffold_setup jquery
      create  public/javascripts/rails_jquery.js
      create  public/javascripts/jquery-ui-timepicker-addon.js
      create  config/initializers/active_scaffold.rb
      insert  app/views/layouts/application.html.erb
      insert  config/locales/en.yml
      gsub  app/views/layouts/application.html.erb
```

Generate active scaffold for resource city & zone
```
rails g active_scaffold City name:string
      invoke  active_record
      create    db/migrate/20110408123206_create_cities.rb
      create    app/models/city.rb
      invoke    test_unit
      create      test/unit/city_test.rb
      create      test/fixtures/cities.yml
      route  resources :cities do as_routes end
      invoke  active_scaffold_controller
      create    app/controllers/cities_controller.rb
      create    app/helpers/cities_helper.rb
      invoke    test_unit
      create    test/functional/cities_controller_test.rb
      create    app/views/cities
```
```
rails g active_scaffold Zone name:string city:references      
      invoke  active_record
      create    db/migrate/20110408123531_create_zones.rb
      create    app/models/zone.rb
      invoke    test_unit
      create      test/unit/zone_test.rb
      create      test/fixtures/zones.yml
       route  resources :zones do as_routes end
      invoke  active_scaffold_controller
      create    app/controllers/zones_controller.rb
      create    app/helpers/zones_helper.rb
      invoke    test_unit
      create      test/functional/zones_controller_test.rb
      create    app/views/zones
```

Add Associations
```
class City < ActiveRecord::Base
 end

 class Zone < ActiveRecord::Base
   belongs_to :city
 end

 class City < ActiveRecord::Base
   has_many :zones
 end
```

Migrate database 
```
rake db:create

rake db:migrate
  ==  CreateCities: migrating ===================================
  -- create_table(:cities)
   -> 0.1411s
  ==  CreateCities: migrated (0.1413s) ==========================

  ==  CreateZones: migrating ====================================
  -- create_table(:zones)
   -> 0.1507s
  ==  CreateZones: migrated (0.1510s) ===========================
```

Start Server 
```
rails s
```

Visit Browser URL: http://localhost:3000/cities

{% img left http://4.bp.blogspot.com/-CJgSXg5EJyE/TZ8EDHwCwaI/AAAAAAAABPs/81dNxWDwYm0/s1600/active_scaffold.png %}
