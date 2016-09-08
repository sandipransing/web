---
layout: post
title: "Grape API authentication using Devise Auth Token"
date: 2014-03-27 02:18:53 +0530
comments: true
categories: Authentication Grape Devise Token
---
In [previous post](http://funonrails.com/2014/03/building-restful-api-using-grape-in-rails/) we saw how to buil RESTful API using Grape. In this post we will see how to add *devise auth token* to *users* and how to use it in *Grape API* authentication.

Lets see how this can be done assuming you already have devise setup ready.
## Add *token_authenticable* to devise modules (works with devise versions <=3.2)
In *user.rb* add *:token_authenticatable* to the list of devise modules, it should look something like below:
```ruby
class User < ActiveRecord::Base
# ..code..
  devise :database_authenticatable,
    :token_authenticatable,
    :invitable,
    :registerable, 
    :recoverable, 
    :rememberable, 
    :trackable, 
    :validatable
  
  attr_accessible :name, :email, :authentication_token
  
  before_save :ensure_authentication_token
# ..code..
end
```
<!-- more -->
## Generate Authentication token on your own (If devise version > 3.2)
```ruby
class User < ActiveRecord::Base
# ..code..
  devise :database_authenticatable,
    :invitable,
    :registerable, 
    :recoverable, 
    :rememberable, 
    :trackable, 
    :validatable
  
  attr_accessible :name, :email, :authentication_token
  
  before_save :ensure_authentication_token
  
  def ensure_authentication_token
    self.authentication_token ||= generate_authentication_token
  end
 
  private
  
  def generate_authentication_token
    loop do
      token = Devise.friendly_token
      break token unless User.where(authentication_token: token).first
    end
  end
```
## Add migration for authentiction token
```
rails g migration add_auth_token_to_users
      invoke  active_record
      create    db/migrate/20140326204628_add_auth_token_to_users.rb
```
Edit migration file to add `:authentication_token` column to users
```
# db/migrate/20140326204628_add_auth_token_to_users.rb
class AddAuthTokenToUsers < ActiveRecord::Migration
  def self.up
    change_table :users do |t|
      t.string :authentication_token
    end

    add_index  :users, :authentication_token, :unique => true
  end
  
  def self.down
    remove_column :users, :authentication_token
  end
end
```
#### Run migrations
```
rake db:migrate
```
#### Generate token for existing users
We need to call save on every instance of user that will ensure authentication token is present for each user.
```
User.all.each(&:save)
```
#### Secure Grape API using auth token
You need to add below code to the *API::Root* in-order to add token based authentication. If you are unware of *API::Root* then please read [Building RESTful API using Grape](http://funonrails.com/2014/03/building-restful-api-using-grape-in-rails/)

In below example, We are authenticating user based on two scenarios
- If user is logged on to the web app then use the same session
- If session is not available and *auth token* is passed then find user based on the token
```ruby
# lib/api/root.rb
module API
  class Root < Grape::API
    prefix    'api'
    format    :json

    rescue_from :all, :backtrace => true
    error_formatter :json, API::ErrorFormatter

    before do
      error!("401 Unauthorized", 401) unless authenticated
    end

    helpers do
      def warden
        env['warden']
      end

      def authenticated
        return true if warden.authenticated?
        params[:access_token] && @user = User.find_by_authentication_token(params[:access_token])
      end

      def current_user
        warden.user || @user
      end
    end

    mount API::V1::Root
    mount API::V2::Root
  end
end
```
