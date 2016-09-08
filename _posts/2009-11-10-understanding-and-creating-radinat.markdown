---
layout: post
title: "Understanding and creating radinat extensions"
date: 2009-11-10T13:52:00+05:30
comments: false
categories:
 - radiant
 - cms
---

## Understanding and creating radinat extensions
To start with, first understand *what is radiant* and *why to use it ?*

> Radiant is a open source content management system designed that serves cms needs for small organisations

## Creating new radint application
```
radiant -d  mysql cms
  create
  create    CHANGELOG
  create    CONTRIBUTORS
  create    INSTALL
  create    LICENSE
  create    README
  create    config
  create    config/environments
  .....
```
Radiant supports extensions. It means one can add extra features to radiant based cms to add extra capabilities. 
Extension directory structure is almost similar to any standard rails application 

## Radiant Extension Directory structure
```
|-- app
       |-- controllers
       |-- helpers
       |-- models
       |-- views
|-- db
       |-- migrate
       |-- seeds.rb
|-- lib
       |-- spec
          |-- controllers
          |-- helpers
          |-- models
          |-- spec.opts
          |-- spec_helper
          |-- views
```
## Creating radiant extension
radiant has generators to create new radint extension
```
script/generate extension ExtensionName
```
Lets create session management extension for radint cms. It will create directory structure for extension.
```
script/generate extension session_management
  create vendor/extensions/session_management/app/controllers
  create vendor/extensions/session_management/app/helpers
  create vendor/extensions/session_management/app/models
  create vendor/extensions/session_management/app/views
  create vendor/extensions/session_management/db/migrate
  create vendor/extensions/session_management/lib/tasks
  create vendor/extensions/session_management/README
  create vendor/extensions/session_management/session_management_extension.rb
  create vendor/extensions/session_management/lib/tasks/session_management_extension_tasks.rake
  create vendor/extensions/session_management/spec/controllers
  create vendor/extensions/session_management/spec/models
  create vendor/extensions/session_management/spec/views
  create vendor/extensions/session_management/spec/helpers
  create vendor/extensions/session_management/features/support
  create vendor/extensions/session_management/features/step_definitions/admin
  create vendor/extensions/session_management/Rakefile
  create vendor/extensions/session_management/spec/spec_helper.rb
  create vendor/extensions/session_management/spec/spec.opts
  create vendor/extensions/session_management/cucumber.yml
  create vendor/extensions/session_management/features/support/env.rb
  create vendor/extensions/session_management/features/support/paths.rb
```

Edit `session_management_extension.rb` where extension version, description, and website url can be added.
```ruby
# require_dependency 'application_controller'
class SessionManagementExtension < Radiant::Extension
  version "1.0"
  description "Describe your extension here"
  url "http://yourwebsite.com/session_management"

  # define_routes do |map|
  # map.namespace :admin, :member => { :remove => :get } do |admin|
  #     admin.resources :session_management
  # end
  # end

  def activate
    # admin.tabs.add "Session Management", "/admin/session_management", :after =>
"Layouts", :visibility => [:all]
  end

  def deactivate
    # admin.tabs.remove "Session Management"
  end
end
```

In activate block, specify what are the library files , modules that needs to be activated while application starts. In my case, activate method looks like below..

```ruby
def activate
  # admin.tabs.add "Session Management", "/admin/session_management", :after =>
"Layouts", :visibility => [:all]
  ApplicationController.send(:include, SessionManagementExt::ApplicationControllerExt)
end
```
## Generating models and controllers
```
script/generate extension_model session_management session_info
session_id:string url:string ip:string
```
In above command first attribute is the extension name and next is model name and rest specifies attributes that needs to created. It will create
```
exists   app/models/
     exists   spec/models/
     create   app/models/session_info.rb
     create   spec/models/session_info_spec.rb
     exists   db/migrate
     create   db/migrate/20091110075042_create_session_infos.rb
```

## Generating controller
```
script/generate extension_controller session_management admin/session_managements
```

It will create an output

```
create app/controllers/admin
     create app/helpers/admin
     create app/views/admin/session_managements
     create spec/controllers/admin
     create spec/helpers/admin
     create spec/views/admin/session_managements
     create spec/controllers/admin/session_managements_controller_spec.rb
     create spec/helpers/admin/session_managements_helper_spec.rb
     create app/controllers/admin/session_managements_controller.rb
     create app/helpers/admin/session_managements_helper.rb
```
Modify session managements controller for displaying sesssion infos tracked.


We need before filter for every request that will capture `session`, `ip` and `page url`. 
so we need to override behaviour of application controller to add before_filter


We have already added `ApplicationControllerExt` in activate of extension.
```
ApplicationController.send(:include, SessionManagementExt::ApplicationControllerExt)
```
Lets look into ApplicationControllerExt module.
```ruby
module SessionManagementExt
  module ApplicationControllerExt
    def self.included(base)
     base.class_eval do
       before_filter :track_session
     end
    end
    def track_session
     #**"Hello from Session tracker !!!"**
     #TODO: location track
     # It can be delayed task
     #sudo gem install geoip_city -- --with-geoip-dir=/opt/GeoIP
     # require 'geoip_city'
     # g = GeoIPCity::Database.new('/opt/GeoIP/share/GeoIP/GeoLiteCity.dat')
     # res = g.look_up('201.231.22.125')
     # {:latitude=>-33.13330078125, :country_code3=>"ARG",
:longitude=>-64.3499984741211, :city=>"Río Cuarto", :country_name=>"Argentina",
:country_code=>"AR", :region=>"05"}
     SessionInfo.create( :ip => request.remote_ip, :page_url =>
"http://#{request.env["HTTP_HOST"]}#{request.request_uri}", :session_id =>
request.session.session_id )
    end
  end
end
```
That's all! Creating rake task for extension module here is default genrated rake task for session management under `vendor/extensions/session_management/lib/tasks/session_management_extension_tasks.rake`. It includes task to migrate database and update extension
```
namespace :radiant do
  namespace :extensions do
    namespace :session_management do
     desc "Runs the migration of the Session Management extension"
     task :migrate => :environment do
       require 'radiant/extension_migrator'
       if ENV["VERSION"]
         SessionManagementExtension.migrator.migrate(ENV["VERSION"].to_i)
       else
         SessionManagementExtension.migrator.migrate
       end
     end
     desc "Copies public assets of the Session Management to the instance public/ directory."
     task :update => :environment do
       is_svn_or_dir = proc {|path| path =~ /\.svn/ || File.directory?(path) }
       puts "Copying assets from SessionManagementExtension"
       Dir[SessionManagementExtension.root + "/public/**/*"].reject(&is_svn_or_dir).each do
|file|
         path = file.sub(SessionManagementExtension.root, '')
         directory = File.dirname(path)
         mkdir_p RAILS_ROOT + directory, :verbose => false
         cp file, RAILS_ROOT + path, :verbose => false
       end
     end
    end
  end
end
```
Add as many custom tasks needed inside this file without changing default tasks.
Migrate all radiant extensions
```
rake db:migrate:extensions
```
