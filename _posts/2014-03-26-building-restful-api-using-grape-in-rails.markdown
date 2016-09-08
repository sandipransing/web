---
layout: post
title: "Building RESTful API using Grape in Rails"
date: 2014-03-26 12:39:27 +0530
comments: true
categories: RESTful API Grape Rails
---
While developing a *rich client side web* application or *mobile app*, we need *RESTful JSON API* which interacts with the *front-end javascript framework*. Here you may use **backbone.js**, **ember.js** or **angular.js** on the front-end side of application.

Here we'll be using *Ruby on Rails* on the *back-end* which will serve *JSON API* consumable by fron-end framework. If you look at the [ruby toolbox](https://www.ruby-toolbox.com/categories/API_Builders) you'll see many API Builder gems available but it seems *grape* can be a good choice.

{% img left no-border https://raw.githubusercontent.com/wiki/intridea/grape/grape_logo.png 186 67 Grape %} *Grape is a RESTful API microframework built to easily and quickly produce APIs for Ruby-rooted web applications.*
<p style='clear:both'>
Let's see how we can build RESTful JSON apis using <i>Grape</i> library:
</p>
<!--more-->
## Getting Started

Add *grape* to your *Gemfile* and then run *bundle* install
```
gem 'grape'
```

## Modularizing API directory structure
Place API files into *lib/api*. You need to create *api* folder inside *lib* directory. 
As we are placing api directory inside lib you don't need to explicitly load it inside *application.rb* 
If you want to place *api* directory at some other place then add below lines to to *application.rb*
```ruby
config.paths.add "app/api", glob: "**/*.rb"
config.autoload_paths += Dir["#{Rails.root}/app/api/*"]
```
First, Let's create *API::Root* class that will mount available api versions.
```ruby
# lib/api/root.rb
module API
  class Root < Grape::API
    prefix 'api'
    mount API::V1::Root
    # mount API::V2::Root (next version)
  end
end
```
Now, create a *API::V1::Root* class that will mount resources for version 1
```ruby
# lib/api/v1/root.rb
module API
  module V1
    class Root < Grape::API
      mount API::V1::Posts
      mount API::V1::Authors
    end
  end
end
```
Now, add resource *Posts* available for api access in json format
```ruby
# lib/api/v1/posts.rb
module API
  module V1
    class Posts < Grape::API
      version 'v1'
      format :json

      resource :posts do
        desc "Return list of recent posts"
        get do
          Post.recent.all
        end
      end
    end
  end
end
```
Now, lets add one more resource `Authors` to version v1
```ruby
# lib/api/v1/authors.rb
module API
  module V1
    class Authors < Grape::API
      version 'v1' 
      format :json 

      resource :authors do
        desc "Return list of authors"
        get do
          Author.all
        end
      end
    end
  end
end
```
## Mounting *API* under rails routes
Mount *API::Root* under routes pointing to rails root
```
# config/routes.rb
SampleApp::Application.routes.draw do
  mount API::Root => '/'
end
```
## Customize JSON API Errors
We can control the api raised errors and customize them so that response is in our own format whenever there are exceptions.
```ruby
# lib/api/error_formatter.rb
module API
  module ErrorFormatter
    def self.call message, backtrace, options, env
      { :response_type => 'error', :response => message }.to_json
    end
  end
end
```
Now, you can plug this module inside *API::Root*
```ruby
# lib/api/root.rb
module API
  class Root < Grape::API
    #...
    error_formatter :json, API::ErrorFormatter
    #...
  end
end
```
You can override error formatter for particular api version. Let's customize errors for *API::v1::Root*:
```ruby
#lib/api/v1/error_formatter.rb
module API
  module V1
    module ErrorFormatter
      def self.call message, backtrace, options, env
        { :response_type => 'error', :response => message }.to_json
      end
    end
  end
end

# lib/api/v1/root.rb
module API
  module V1
    class Root < Grape::API
      #...
      error_formatter :json, API::V1::ErrorFormatter
      #...
    end
  end
end
```
## Accessing *API* routes
If you do `rake routes | grep api` then it will list only mount path for api but do not list all the paths.
```
rake routes | grep api
    api_root        /api                API::Root
```
So, in-order to list all api paths, you may have to create api routes task:
```ruby
# lib/tasks/routes.rake
namespace :api do
  desc "API Routes"
  task :routes => :environment do
    API::Root.routes.each do |api|
      method = api.route_method.ljust(10)
      path = api.route_path.gsub(":version", api.route_version)
      puts "     #{method} #{path}"
    end
  end
end
```
Now, run task and it should print routes like this:
```
rake api:routes
    GET        /api/v1/posts(.:format)
    GET        /api/v1/authors(.:format)
```
## Securing API
Now we have got Grape *API* ready and working properly. Lets see how we can secure *API*. There are many approaches to authenticate API. Here lets first get it working with simple *HTTP Basic authentication*. 

#### HTTP Basic authentication
In our case, lets add basic authentication to the *API::Root* and it will get applied to all versions of API.
```ruby
# lib/api/root.rb
module API
  class Root < Grape::API
    #...
    
    http_basic do |email, password|
      user = User.find_by_email(email)
      user && user.valid_password?(password)
    end
    #...
  end
end
```
Requesting API using basic http auth credentials:
```
curl http://localhost:3000/api/products -u "admin:secret"
```
#### Authenticate using email and password
Grape provides us with *before block* inside that we can add authenctication code.
```ruby
# lib/api/root.rb
module API
  class Root < Grape::API
    #...
    before do
      error!("401 Unauthorized", 401) unless authenticated
    end
    
    helpers do
      def authenticated
        user = User.find_by_email(params[:email])
        user && user.valid_password?(params[:password])
      end
    end
    #...
  end
end
```
