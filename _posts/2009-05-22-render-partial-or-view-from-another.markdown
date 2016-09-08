---
layout: post
title: "Render partial or view from another controller"
date: 2009-05-22T19:50:00+05:30
comments: false
categories:
 - rails
 - views
---
To render view files from another controller's view files

## In rail 2.3
```
render "controller/action"
```

## In rails 2.2 or below
```
render :template => 'controller/action'
```

## To render partial from another controller’s views folder
```
render :partial => "controller/partial"
```
