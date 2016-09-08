---
layout: post
title: "Awesome: Working with the Rails console and all the tricks"
date: 2009-05-08T16:03:00+05:30
comments: false
categories:
 - rails
---

Testing your active record methods using *script/console*

#### Windows
If you are using windows, you start the console by using this command:
```
ruby script/console
```

#### Linux:
```
./script/console
```
Just use the following command whenever you make changes to your model objects:

```
reload!
```
Instead of accessing your MySQL database with
```
mysql -u  -p
```

You can instead do
```
script/dbconsole
```

and if you database has a password, just do
```
ruby script/dbconsole -p
```
Connecting to different rails environments
```
ruby script/dbconsole     # connect to development database (or $RAILS_ENV)
ruby script/dbconsole production    # connect to production database
```
