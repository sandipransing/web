---
layout: post
title: "Getting started with rails 3 & postgres database"
date: 2011-03-01T00:40:00+05:30
comments: false
categories:
 - rails 3
 - postgres
---

Rails 3 Installation
```
sudo gem install rails -v3.0.4
```

Postgres as `DB` installation
```
sudo apt-get install postgresql
```

Rails 3 App with postgres as database
```
rails new pg -d postgres
```

Bundle installation
It will install dependency gems & postgres adapter for db connection
```
bundle install
```

Here by default `postgres` database user gets created while installation but i recommend to create new db user with name same as of system user owning project directory permissions. 

User owning file permission can be found using
```
ls -l pg
drwxr-xr-x 7 sandip sandip 4096 2011-02-23 15:38 app
drwxr-xr-x 5 sandip sandip 4096 2011-02-23 18:14 config
-rw-r--r-- 1 sandip sandip  152 2011-02-23 15:38 config.ru
...
```

Default 'postgres' database user gets created while installation
```
## Snap of database.yml
development:
  adapter: postgresql
  encoding: unicode
  database: pg_development
  username: sandip
  pool: 5
```

Create new database user
```
pg $ sudo su postgres
pg $ createuser sandip
Shall the new role be a superuser? (y/n) y
pg $ exit
exit
```

Create a first development database:
```
pg $ psql template1
Welcome to psql 8.4.6, the PostgreSQL interactive terminal.
...

template1=# \l
        List of databases
   Name    |  Owner   | Encoding 
-----------+----------+----------
 postgres  | postgres | UTF8
 template0 | postgres | UTF8
 template1 | postgres | UTF8
(3 rows)

template1=# CREATE DATABASE pg_development;
CREATE DATABASE
template1=# \l
            List of databases
       Name        |  Owner   | Encoding 
-------------------+----------+----------
 postgres          | postgres | UTF8
 pg_development | sandip   | UTF8
 template0         | postgres | UTF8
 template1         | postgres | UTF8
(4 rows)

template1=# \q
```

Start rails server
```
pg $ rails s
```

### Getting hands on postgres terminal
**1.  Login onto terminal**
```
psql -U DB_USERNAME -d DB_NAME -W DB_PASSWORD
```
**2.  List databases**
```
\l
```
**3.  Display tables**
```
\dt
```
**4.  Exit from terminal**
```
\q
```
