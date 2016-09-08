---
layout: post
title: "Solution to DelayedJob(DJ) gem server start problem"
date: 2010-10-05T16:15:00+05:30
comments: false
categories:
 - delayed-job
 - rails
---

Solution to DelayedJob(DJ) gem server start problem 

I had installed delayed_job gem 2.0.3, daemons gem but after staring DJ server it shows daemon started but actually process gets killed automatically. 

I performed steps given by Kevin on google group and it worked like charm 

Here are the steps:
## 1 Add github to gem sources
```
sudo gem sources -a http://gems.github.com
```
## 2 Install alexvollmer-daemon-spawn
```
sudo gem install alexvollmer-daemon-spawn
```
## 3 Move the old daemons delayed job script out of the way
```
mv script/delayed_job script/delayed_job.daemons
```
## 4 Copy below gist to *script/delayed_job*: 
<script src="https://gist.github.com/akmathur/104314.js"></script>


Try it out again making sure it writes to the tmp/pids directory ok.

My line looks like this:
```
RAILS_ENV=production script/delayed_job start
```
then to check (besides running 'ps'), you can run this:
```
RAILS_ENV=production script/delayed_job status
```
