---
layout: post
title: "using rails acts as taggable on plugin"
date: 2009-05-08T14:22:00+05:30
comments: false
---

A tagging plugin for Rails applications that allows for custom tagging along dynamic contexts.

## Plugin Installation
```
ruby script/plugin install git://github.com/mbleigh/acts-as-taggable-on.git
```

## Install as a Gem
```
gem install mbleigh-acts-as-taggable-on --source http://gems.github.com
```

## Auto Install
```
# include following line in environment.rb
config.gem "mbleigh-acts-as-taggable-on", :source => "http://gems.github.com", :lib => "acts-as-taggable-on"
rake gems:install
```

## Post Installation (Rails)
```
ruby script/generate acts_as_taggable_on_migration
rake db:migrate
```

## Adding taggable to model
Add following line in your model for which you wanted to be tagged
```
acts_as_taggable_on :tags
```

## Example
Lets take `post` model

```ruby
class Post < ActiveRecord::Base
  acts_as_taggable_on :tags
end
```

## Controller code
Inside controller action
```ruby
@post = Post.new(params[:post])
# Or just hardcode to test
@post.tag_list ="awesome, slick, hefty"   
@post.save
```

## Methods
What are the methods provided?
```ruby
@post.tag_list = "awesome, slick, hefty"   
Post.tagged_with("awesome", :on => :tags)
# => [@post1, @post2]

Post.tag_counts # => [,...]
```

## Named Scope
```ruby
class Post
end

# created_at DESC
Post.tagged_with("awesome").by_creation
```

## Pagination
It is easy to paginate tags collection using will_paginate plugin

```ruby
List_Of_Tags.paginate(:page => params[:page], :per_page => 20)
```

```ruby
@post.find_related_tags
# => will give related posts having related tags. [ @post1, @post2, ...]
```

## Tag Owners
```ruby
class User < ActiveRecord::Base
  acts_as_tagger
end

class Post < ActiveRecord::Base
  acts_as_taggable_on :tags
end
```
## Console play
```ruby
@some_user.tag(@some_post, :with => "paris, normandy", :on => :tags)
@some_user.owned_taggings
@some_user.owned_tags
@some_post.tags_from(@some_user)
```
