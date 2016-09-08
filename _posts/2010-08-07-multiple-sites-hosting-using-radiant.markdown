---
layout: post
title: "Multiple sites hosting using radiant cms"
date: 2010-08-07T20:49:00+05:30
comments: false
categories:
 - radiant
 - cms
---

This involves 4-5 simple in-order to get multiple sites working using radiant cms. Follow the below steps one by one.
#### Setup radiant cms
first Install radiant gem

```
sudo gem install radiant
```
set 'radiant' under path

```
export PATH=$PATH:/var/lib/gems/1.8/gems/radiant-0.8.0/bin
```

Now, As we are done with radiant installation lets create new project.

```
$ radiant multisite -d mysql
```

#### Edit database config

```
$ cd multisite/
$ vi config/database.yml
development:
  adapter: mysql
  database: multisite_development
  username: root
  password: abcd
  host: localhost
```

#### Migrations
```
rake db:create
```

#### Run the database bootstrap rake task:
```
rake db:bootstrap
This task will destroy any data in the database. Are you sure you want
to continue? [yn] yes
...
Create the admin user (press enter for defaults).
Name (Administrator): admin
Username (admin): admin
Password (radiant): 
....
Select a database template:
1. Empty
2. Roasters (a coffee-themed blog or brochure)
3. Simple Blog
4. Styled Blog
[1-4]: 2
....
```
#### Now we are ready to start

```
ruby script/server
```

Open following URL in browser:

`http://localhost:3000`

You should see site homepage. To manage site go to admin panel.
`http://localhost:3000/admin`

Now Lets start with multiple sites management

**1) Clone multi_site extension into `vendor/extensions` of your project**
```
  cd vendor/extensions/
  git clone git://github.com/zapnap/radiant-multi-site-extension.git multi_site
```
**2) Run the extension migrations**
```
  rake db:migrate:extensions
```

**3) Run the extension update task**
```
  rake radiant:extensions:multi_site:update
```
**4) Restart your server**

This finishes our multisite installation. Open following URL in browser.

`http://localhost:3000/admin/sites`

Add multiple sites as you wanted
```
 Name        Domain pattern     Base domain name

================================================

 site 1      sandip             sandip

 site 2      gautam             gautam
```
 
Don't forget to make dns entries in `/etc/hosts`

```
# /etc/hosts
 
127.0.0.1 localhost
...
127.0.0.1 sandip
127.0.0.1 gautam
```
One more, by deault pages created for new sites added are not published.
Please be sure to make them published and modify contents to identify them


Now you can view different sites:

1.  http://sandip:3000
2.  http://gautam:3000
