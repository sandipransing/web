---
layout: post
title: "has_many => through association for rails polymorphic model"
date: 2010-02-15T21:54:00+05:30
comments: false
categories:
 - associations
 - rails
---

Example of polymorphic association using has_many, through, source

Here post is polymorphic resource which belongs to the author [can be student, teacher]

```ruby
# app/models/student.rb
class Student < ActiveRecord::Base

  has_many :posts, :as => author
end

# app/models/teacher.rb
class Teacher < ActiveRecord::Base

  has_many :posts, :as => author
end

# app/models/post.rb
class Post < ActiveRecord::Base

  belongs to :author, :polymorphic => true
end
```
Has_many through association on polymorphic models

```ruby
# app/models/student.rb
class Student < ActiveRecord::Base

  beloyngs_to :division
  has_many :posts, :as => author
end

# app/models/division.rb
class Division < ActiveRecord::Base

  has_many :students
  has_many :student_posts, :through => :students, :source => :posts
end
```
Examples 

```ruby
ruby script/console
div = Division.first
div.student_posts
```
