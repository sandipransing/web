---
layout: post
title: "Page Cache control in radiant cms"
date: 2010-03-04T00:43:00+05:30
comments: false
categories:
 - radiant
 - cms
---

## Radiant Caching

Radiant cms is very powerful and customizable cms as of now which has inbuilt support for page caching.

Radiant caching mechanisam is somehow similar to action caching in rails.

In latest radiant version i.e. > 0.8 Responsecache has been replaced with Radiant::Cache.

By default radiant cache gets automatically invalidated after every 5 minutes and that is configurable.

The interval is easily configurable by adding following lines inside environment
```ruby
if defined? ResponseCache == 'constant'    
  ResponseCache.defaults[:expire_time] = 4.hours
else
  SiteController.cache_timeout = 4.hours
end
```
There are situations where automatic cache inavalidation won't work and we need to clear radiant cache on the fly.
There are two ways to do that to invalidate radiant cache immediately.

## 1. Navigate to the root of your Radiant project and delete the cache directory
```
cd /home/deploy/radiant_site/tmp
rm -r cache
```

## 2. Clearing the page cache from within your code

```ruby
if defined? ResponseCache == 'constant'
  ResponseCache.instance.clear
else
  Radiant::Cache.clear
end
```
While building website using radiant cms, it happens that there are certain pages they are static and not going to change frequently that time configuring cache expiry time to long interval is going to be always beneficial and for pages which conatins dynamic content (displaying logged in user on homepage), we need to disable radiant cache for such pages. this can be done by using page_options extension.

## Installation for radiant version  0.7

From your RADIANT_ROOT:

```
ruby script/extension install page_options
```
## Installation for radiant version  0.8 and higher
```
git clone git://github.com/sandipransing/radiant-page_options-extension.git vendor/extensions/page_options
```

Restart server

## Usage

1. Goto /admin/pages
2. Edit any page
3. Click on more link and edit cache settings.

For more information [visit](http://github.com/avonderluft/radiant-page_options-extension)
