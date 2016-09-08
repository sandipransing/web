---
layout: post
title: "Multiple resources, registrations with devise, STI and single sign sign on"
date: 2011-12-22T02:51:00+05:30
comments: false
categories:
 - inheritance
 - rails 3
 - devise
---
> Devise handles authentication, authorization part inside rails application quite easily and its customizable too. One can always customize default devise configurations. 

This Post will show how to manage multiple resources (like admin, staff, employees, guests etc.) through devise and STI with individual registrations process but login section will be the same for all. 

```
# Gemfile
gem 'devise'
```
Bundle install, generate devise files, devise user, and migrate devise database
```
bundle install
rails g devise_install
rails g devise User
rake db:migrate
rake routes
```
User Model
```ruby
class User < ActiveRecord::Base
  # Include default devise modules. Others available are:
  # :token_authenticatable, :lockable, :timeoutable, :confirmable and :activatable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable

  # Setup accessible (or protected) attributes for your model
  attr_accessible :email, :password, :password_confirmation
end
```

### Single Table Inheritance

Create `Admin`, `Staff`, `Employee`, and `Guest` models inherited from `User` model
```ruby
# Admin
class Admin < User
end

# Staff
class Staff < User
end

# Employee
class Employee < User
end

# Guest
class Guest < User
end
```

Add routes for admin, staff, employee, and guest users
```ruby
# routes
devise_for :users, :skip => :registrations
devise_for :admins, :skip => :sessions
devise_for :staffs, :skip => :sessions
devise_for :employees, :skip => :sessions
devise_for :guests, :skip => :sessions
```

Customizing deafult `routes` and `views`

```ruby
# config/routes.rb
# customizing default login/logout routes, views, actions
devise_for :users, :controller => {:sessions  => 'sessions'}, :skip => [:sessions, :registrations] do
    delete '/logout', :to => 'sessions#destroy', :as => :destroy_user_session
    get '/login', :to => 'sessions#new', :as => :new_user_session
    post '/login', :to => 'sessions#create', :as => :user_session
end

# app/controllers/sessions_controller
class SessionsController < Devise::SessionsController
end
```

**Overriding default after sign in path**
```ruby
# app/controller/application_controller.rb
class ApplicationController < ActionController::Base
  protect_from_forgery
  helper_method :account_url

 def account_url
    return new_user_session_url unless user_signed_in?
    case current_user.class.name
    when 'Customer'
      edit_customer_registration_url
    when 'Admin'
      edit_home_page_section_url
    else
      root_url
    end if user_signed_in?
  end
end

# app/controllers/sessions_controller.rb 
class SessionsController < Devise::SessionsController

  def after_sign_in_path_for(resource)
    stored_location_for(resource) || account_url
  end
end
```

**Changing default login field email to username**
```ruby
# config/initializers/devise.rb 
config.authentication_keys = [ :username ]

# app/models/user.rb
validates :username, :presence => true, 
  :uniqueness => {:allow_blank => true},
  :format => {:with => /^\w+[\w\s:?']+$/i, :allow_blank => true}

def email_required?
  false
end
```
Adding devise authentication and authorization helper methods for above resources. more information on this can be read [here](http://www.funonrails.com/2011/12/dynamic-methods-inside-ruby-classes)
