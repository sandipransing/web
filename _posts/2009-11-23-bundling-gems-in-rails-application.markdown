---
layout: post
title: "bundling gems in rails application"
date: 2009-11-23T14:47:00+05:30
comments: false
categories:
 - bundler
 - rails
---
Gem Bundling is basically used to maintain all required gems at your application level and its very easy to use.
It downloads necessary gems and maintain it under `/vender/gems` directory.

## Install gemcutter
gemcutter provides hosting for ruby gems
```
gem install gemcutter
```
## Install bundler
bundler is a tool that manages gem dependencies for your ruby application
```
gem install bundler
```
## Start using 
```
gem bundle
```
