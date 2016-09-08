---
layout: post
title: "Installing and running cronjob (crontab basics)"
date: 2011-01-20T13:50:00+05:30
comments: false
categories:
 - linux
 - crontab
---

Basic handy commands while working with crontabs
 
## Editing cron file
```
crontab -e
```

## Displaying cron file
```
crontab -l
```

## Removing cron file
```
crontab -r
```
## Cron syntax

> cron command basically takes 6 input parameters of which each input can take multiple arguments for that one can make use of comma or pipe separator

```
# min hour dom mon dow   command
*   *   *   *   * (command)
```

Here \* means for every. Above command will get executed for every minute

## Detail syntax
```
*     *     *   *    *        command to be executed
-     -     -   -    -
|     |     |   |    |
|     |     |   |    +----- day of week (0 - 6) (Sunday=0)
|     |     |   +------- month (1 - 12)
|     |     +--------- day of month (1 - 31)
|     +---------- hour (0 - 23)
+------- min (0 - 59)
```

## Example commands
```
# Generate progressive report for every active user at 11:45 p.m.
45 23 * * * (cd /home/sandip/current/; rake ace:daily_progressive_report RAILS_ENV=production)

# Take database backup every night
59 2 * * * (cd /home/sandip/current/; rake db:backup RAILS_ENV=production)

# Initialize grouping on database every sunday
29 3 * * 0 (cd /home/sandip/current/; rake ace:initializeCarGrouping RAILS_ENV=production)

# Generate daily stats on 4:30 AM & 12:30 PM and 4:30 PM
30 4,12,16 * * * (cd /home/sandip/current/; rake ace:daily_call_statistics RAILS_ENV=production)

# Rotate sphinx indexes updated evry hour if missed out somehow
0 * * * * (cd /user/local/bin/; indexer --rotate --config /home/sandip/current/config/production.sphinx.conf)
```
