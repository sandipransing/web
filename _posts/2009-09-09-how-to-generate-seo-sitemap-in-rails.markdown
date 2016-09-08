---
layout: post
title: "How to generate SEO sitemap in rails ?"
date: 2009-09-09T21:44:00+05:30
comments: false
categories:
 - seo
 - rails
---
## What is sitemap ? 

Sitemap is nothing but a .xml file containing urls available on your website

It contains URL, last modified date, frequency of content change and priority ( between 0..1 )

## Why we need sitemap ? 

We can submit sitemap file to search engine. It will help them in analysing what urls on your site are available for crawling.

## What is xml pattern ? 
```xml
   <?xml version="1.0" encoding="UTF-8"?>
      <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">

        <url>
           <loc>http://www.example.com/</loc>
           <lastmod>2005-01-01</lastmod>
           <changefreq>monthly</changefreq>
           <priority>0.8</priority>
        </url>
       ...
       ...
    </urlset>
```
## What will be the path for sitemap ? 

**"www.example.com/sitemap.xml"**

#### Are we going to generate sitemap manually ? 

No... there is `mephisto sitemap plugin` and you can make can use it.
```
ruby script/plugin install http://svn.appelsiini.net/svn/rails/plugins/mephisto_sitemap/
```
#### Am i needed to generate to sitemap for each request ? 

No... we can generate it in background task daily basis.

#### How it will be accessed for request to sitemap ? 

We can define path to local file in routes

```
map.connect "sitemap.xml", :controller => :sitemap
```

```ruby
class SitemapController
  def index
    render "some local file path"
  end
  ...
```

Here are some reference links :

1.  http://www.fortytwo.gr/blog/19/Generating-Sitemaps-With-Rails
2.  http://www.bestechvideos.com/2008/07/04/ruby-plus-71-how-to-create-a-seo-sitemap-for-rails-apps

## Comments

**info ternak**

i got lots of information from your site, thank for your sharing… :)

**Elena Morgan**

Search engines generate nearly 90% of all Internet traffic and are responsible for 55% of all E-commerce transactions. Today, it is essential for all online businesses to make SEO an integral part of their online business strategy.

So I would like to recommend one of my client who helps you define, evolve and implement a powerful SEO strategy to leverage your online business potential

http://www.redalkemi.com/

**sandipransing**

Link mentioned is very good !
but it has limitation that we will not have any control over urls that will be submitted to search engines :)

**Sethupathi**

*How to create sitemap.xml*

From the following link you can generate the site-map.xml if your application is not in https. Just by giving the application link it will generate the xml for you.

http://www.xml-sitemaps.com/
