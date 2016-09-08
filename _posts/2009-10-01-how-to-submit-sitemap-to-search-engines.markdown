---
layout: post
title: "How to submit sitemap to different search engines"
date: 2009-10-01T22:57:00+05:30
comments: false
categories:
 - seo
 - rails
---

There are many ways that can be used to submit sitemap to search engines.

Lets discuss approaches :

#### 1. Put sitemap.xml in public folder and you are done

Crawlers will be finding `sitemap.xml` file through url
http://yourdomain.com/sitemap.xml

#### 2. There are many online sites which generates sitemaps for you

http://www.xml-sitemaps.com/

#### 3. Open following urls in browser
Make sure to add `sitemap url` of your website in-place of `XML_SITEMAP_PATH` in the below urls

**a. Google submit**

http://www.google.com/webmasters/tools/ping?sitemap=XML_SITEMAP_PATH

**b. Yahoo Submit**

http://search.yahooapis.com/SiteExplorerService/V1/ping?sitemap=XML_SITEMAP_PATH

**c. Ask Submit**

http://submissions.ask.com/ping?sitemap=XML_SITEMAP_PATH

**d. Webmaster submit**

http://webmaster.live.com/ping.aspx?siteMap=XML_SITEMAP_PATH

#### 4. Best practice in rails to submit sitemap i.e. use of scheduled task (cron job)

Write a rake task that will perform following actions:

1.  Generate sitemap in rails public folder
2.  Submit `sitemap.xml` file to search engines

Submitting sitemaps is already explained above and you just need to invoke above urls for each engine
