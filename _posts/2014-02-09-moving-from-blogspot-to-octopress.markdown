---
layout: post
title: "Migrating from blogspot to octopress"
date: 2014-02-09 19:00:56 +0530
comments: true
categories: octopress, blogger
---
I was happily writing my blog at `blogspot` almost three plus years. From last 2-3 days, i was evaluating with [octopress](http://octopress.org) for its way of writing posts, customizations, themes, and plugins. As a result of evaluation, found `octopress` blog as very quick & easy to write hence decided to  *shift* from *blogger* to *octopress*.

Below post explains steps involved in migration:

## Export content from blogger
First, you need to export your posts from blogger.
Goto `blogger admin area` and then go to `settings >> other >> Export Blog` and click on it. Please wait sometime and popup will comeup allowing you to download blog in `xml` format.

<img src="{{ root_url }}/images/export-blog.png" />

If you still finding some problem with exporting then see [here](http://www.freetech4teachers.com/2013/01/how-to-export-your-blogger-and.html)
<!--more-->

## Formatting posts for octopress
Now the big challenge ! We have `xml posts` downloaded from blogger but octopress posts format is different.
So, we need to convert xml posts to markdown formatted octopress posts.
If you do it manually then its not an easy task but don't worry. There is already a ruby code that will do it for you.
Thanks to [Reinaldo de Souza ](https://gist.github.com/juniorz) for sharing the script. Download the `import.rb` script from [here](https://gist.github.com/baldowl/1578928)

## Run the script against downloaded xml file
Script requires `nokogiri`. if you not don't have already then install using `gem install nokogiri`
After successful run, this will be creating static markdown octopress formatted posts inside `_posts` and `_drafts` directories.
```
ruby import.rb blog-02-09-2014.xml
```
## Copy generated posts to octopress blog posts
```
mv _posts/* blog/_posts/`
mv _drafts/* blog/_drafts/`
```
This finishes migration and run `rake preview` and you will start seeing your blogger posts.
