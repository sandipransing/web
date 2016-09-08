---
layout: post
title: "How to map irregular database tables with rails models"
date: 2009-10-21T23:53:00+05:30
comments: false
categories:
 - rails
 - active record
---
Rails specifies conventions while creating models, controllers, migrations ( database tables ). 

## Conventions for creating database tables 

1.  Table name should be plural 
2.  id field should be primary_key for table. 
3.  foreign_key should be like _id i.e. post_id, site_id 

## Conventions for creating models & Controllers 

1.  Model name should be singular 
2.  controller name should be plural. 

Although rails has standard defined, In some cases it becomes quite necessary to map irregular 

#### 1. Table mapping 
```ruby
class Review < ActiveRecord::Base   
  # Here comments is name of database table   
  set_table_name :comments 
end 
```

#### 2. Set primary key ( other than rails standar ‘id’ primary_key )
```ruby
class Review < ActiveRecord::Base   
  # It assumes "reviews" as table in database   
  # Below line indicates reviews table contains column_name "reviewId" which is a primary_key   
  set_primary_key :reviewId 
end 
```

#### 3. Foreign key association 
```ruby
class Review < ActiveRecord::Base   
  # Below line indicates reviews table contains column_name 'RatingId' which is foreign_key to primary_key   
  # of 'ratings' table.   
  # It can be applied to any associan type [ has_one, has_many, belongs_to, has_many_through ]   
  belongs_to :rating, :class_name => "SiteUser", :foreign_key => "RatingId" 
end 
```
#### 4. Model association ( non-standard model name )
```ruby
class Review < ActiveRecord::Base
  # Below line indicates reviews table contains column_name 'UserId' which is foreign_key to primary_key   
  # of model with class_name 'SiteUser'   
  belongs_to :user, :class_name => 'SiteUser', :foreign_key: => 'UserId' 

end 
```
