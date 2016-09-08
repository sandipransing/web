---
layout: post
title: "Best practices to use named scopes inside models"
date: 2011-12-22T18:50:00+05:30
comments: false
categories:
 - rails
 - active record
 - named-scopes
---

> Scopes are partial query conditions to database queries. scopes are always prefixed to class finders. There are several ways to use scopes inside rails models.

**1.  Scope defined below gets loaded while class definition loads**
```ruby
scope :active, where(:active => true)
scope :archived, where(:archived => true, :post_type => :general)
```
**2.  Dynamic scopes needs to be always defined inside lambda**
```ruby
scope :not_expired, lambda { where('expiry_date <= ?', Date.today) }
```
**3.  Combining scopes**
```ruby
scope :visible, published.not_expired
```
**4.  Passing parameters to scopes**
```ruby
# avoid below
scope :created_by_user, lambda {|user|
  where('user_id = ?', user)
}

# use this
scope :created_by_user, lambda {|user|
  where(:user_id => user)
}
```

**5.  passing multiple parameters**
```ruby
# avoid below
scope :made_between, lambda{|from, to|
  where('created_date >= ? and created_date <= ?', from, to)
}
# use this
scope :made_between, lambda{|from, to|
  where('created_date >= :from and created_date <= :to', :from => from, :to => to)
}
```

**6.  associations inside scope (joins and includes)**
```ruby
# below will perform eager loading effective when rendering posts with comments
scope :with_user_comments, lambda{|user|
  includes(:comments).where('comments.user_id = ?', user)
}

# faster
# also can be done as post.comments.where(:user_id => user)
scope :with_user_comments, lambda{|user|
  joins(:comments).where('comments.user_id = ?', user)
}
```

So, at last would suggest making use of symbols when there are multiple parameters to scopes and make maximum use of scopes rather than having where conditions everywhere :)
