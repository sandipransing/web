---
layout: post
title: "active-admin sass and rails 3"
date: 2011-12-28T14:58:00+05:30
comments: false
categories:
 - rails 3
 - sass
 - activeadmin
---
> Active Admin is the good way to provide rails administrative interface. It provides front-end db administration and its customizable too :)

```
# Gemfile
gem 'activeadmin'
gem 'sass-rails'
gem "meta_search",    '>= 1.1.0.pre'
```

Bundle install, generate config & migrate db 
```
bundle install
rails g active_admin:install
rake db:migrate
```

Config 
```ruby
# config/initializers/active_admin.rb 
ActiveAdmin.setup do |config|
  config.site_title = "Web Site :: Admin Panel"
  config.site_title_link = "/"
  config.default_namespace = :siteadmin
  config.authentication_method = :authenticate_admin_user!
  config.current_user_method = :current_admin_user
  config.logout_link_method = :delete
end
```

Registering new resource 
```
rails generate active_admin:resource category
```

Customization
```ruby
# app/admin/categories.rb 
ActiveAdmin.register Category do
  scope :published

  form do |f|
    f.inputs do
      f.input :name, :label => 'Name'
      f.input :for_type, :label => "Category Type"
    end
    f.buttons
  end
end
```

Adding Dashboard
```
ActiveAdmin::Dashboards.build do
  section "Recent Categories" do
    table_for Category.published.recent.limit(2) do
      column :name do |c|
        link_to c.name, [:admin, c]
      end
      column :created_at
    end
    strong { link_to "View All Categories", admin_categories_path }
  end
end
```
