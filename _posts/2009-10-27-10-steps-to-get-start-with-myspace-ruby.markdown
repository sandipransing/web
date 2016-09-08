---
layout: post
title: "10 steps to get start with MySpace Ruby SDK"
date: 2009-10-27T21:33:00+05:30
comments: false
categories:
 - api
 - rails
 - myspace
---
Follow 10 simple steps listed below in order to use myspace sdk api

#### 1. Remove existing `myspace` gems installed if any
```
gem uninstall myspace
```

#### 2. Checkout sample source code
```
svn checkout http://myspaceid-ruby-sdk.googlecode.com/svn/trunk/ myspacesdk
```

#### 3. Move to sample dir
```
cd myspacesdk/samples/rails/sample
```

#### 4. Modify DB config
```yml
# Modify config/database.yml accordingly
development:
  adapter: mysql
  database: sample_development
  password: abcd
  pool: 5
  timeout: 5000
```

#### 5. Download Gem
```
wget http://myspaceid-ruby-sdk.googlecode.com/files/myspaceid-sdk-0.1.11.gem
```

#### 6. Install `myspaceid-sdk` gem locally
```
gem install --local ~/Desktop/myspaceid-sdk-0.1.11.gem 
# --local option taskes local PATH_TO_GEM
```

####  7. Above command supposed to give following error otherwise skip to step 10
```
ERROR:  Error installing /home/sandip/Desktop/myspaceid-sdk-0.1.11.gem:
myspaceid-sdk requires ruby-openid (>= 0, runtime)
```

#### 8. Install `ruby-openid` and go to "step 6"
```
gem install ruby-openid
# Go to step 6
```

####  9. Start Server
```
ruby script/server
```

####  10.  Browse Application
Open URL http://localhost:3000/
