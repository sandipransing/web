---
layout: post
title: "Writing Octopress blog posts using markdown"
date: 2014-02-07 19:20:07 +0530
comments: false
author: Sandip Ransing
published: true
categories: octopress 
---
This is an example post which shows various markdown syntax that can be used while writing octopress markdown posts.
## Bold text
**Hello Octopress!**

## Blockquote
{% blockquote @TheJeetBanerjee http://twitter.com/TheJeetBanerjee %}
Don't stop when you're tired, stop when you're done.
{% endblockquote %}
<!--more-->
{% blockquote @Vivek Paul %}
We have to go for what we think we're fully capable of, not limit ourselves by what we've been in the past.
{% endblockquote %}

## Ruby
```ruby
class A
  def abc
    puts 'hii'
  end
end
```

## Console
```
$ sudo apt-get install cakePHP
```
```
$ git clone git@github.com:imathis/octopress.git # fork octopress
```

## Coffeescript
```coffeescript
$('.help').html "(?)"
console.log "print something"
# sum function
sum = (a, b) ->
  a + b

```

## Gist Embedding
{% gist cc14d6039f1ff35b4be3  multiple-ssh-config %}

## Include Code Snippets
```
{% include_code path/to/file [title] [lang:language] [start:#] [end:#] [range:#-#] [mark:#,#-#] [linenos:false] %}
```
## Selecting tags from sentence
This is a `ruby` and `rails` blog

## Pullquote
## Images
{% img http://placekitten.com/890/280 %}


## AngularJS Tutorial - YouTube
AngularJS is a client-side JavaScript framework.A video tutorial to help you get started with AngularJS.
[![Angular JS](http://img.youtube.com/vi/WuiHuZq_cg4/0.jpg)](http://youtube.com/watch?v=WuiHuZq_cg4)

## Inline HTML
```
<div class='well'>
  All is well. Way to go !!
</div>
```
