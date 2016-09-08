---
layout: post
title: "Monitor Delayed Job in rails"
date: 2011-03-01T01:22:00+05:30
comments: false
categories:
 - delayed-job
 - monit
 - rails
---

## Delayed Job & Monit configuration 

We were struggling through how to monit delayed_job from past few months because monit doesn't work seamlessly with delayed_job start/stop commands and finally we got able to monit delayed_job. 

Here is our old configuration that wasn't working anyhow-

```
check process delayed_job with pidfile /home/sandip/shared/pids/delayed_job.pid
    stop program = "/bin/bash -c 'cd /home/sandip/current && RAILS_ENV=production script/delayed_job stop'"
    start program = "/bin/bash -c 'cd /home/sandip/current && RAILS_ENV=production script/delayed_job start'"
    if totalmem > 100.0 MB for 3 cycles then restart
    if cpu usage > 95% for 3 cycles then restart
```
After doing google & looking at stackoverflow, we found different solutions to work with but none of them found useful to me. :( 

After reading google group someone (not remembering exactly) directed to write a init script for delayed_job server and that perfectly worked for me and my headache of self moniting delayed_job ended up ;)

#### Delayed Job init script

*/etc/init.d/delayed_job*
```
#! /bin/sh
set_path="cd /home/sandip/current"

case "$1" in
    start)
    echo -n "Starting delayed_job: "
    su - root -c "$set_path; RAILS_ENV=production script/delayed_job start" >> /var/log/delayed_job.log 2>&1
    echo "done."
    ;;
    stop)
    echo -n "Stopping delayed_job: "
    su - root -c "$set_path; RAILS_ENV=production script/delayed_job stop" >> /var/log/delayed_job.log 2>&1
    echo "done."
    ;;
    *)
    echo "Usage: $N {start|stop}" >&2
    exit 1
    ;;
esac

exit 0
```

finally here is the working monit delayed_job configuration

```
check process delayed_job with pidfile /home/sandip/shared/pids/delayed_job.pid
    stop program = "/etc/init.d/delayed_job stop"
    start program = "/etc/init.d/delayed_job start"
    if totalmem > 100.0 MB for 3 cycles then restart
    if cpu usage > 95% for 3 cycles then restart
```

#### Thinking Sphinx monit configuration
```
check process sphinx with pidfile /home/sandip/shared/pids/searchd.pid
    stop program = "/bin/bash -c 'cd /home/sandip/current && /usr/bin/rake RAILS_ENV=production ts:stop'"
    start program = "/bin/bash -c 'cd /home/sandip/current && /usr/bin/rake RAILS_ENV=production ts:start'"
    if totalmem > 85.0 MB for 3 cycles then restart
    if cpu usage > 95% for 3 cycles then restart
```

#### Adhearsion (ahn) monit confiuration
```
check process ahn with pidfile /home/josh/shared/pids/ahnctl.pid
    stop program = "/bin/bash -c 'cd /home/sandip/current && /usr/bin/ahnctl stop adhearsion'"
    start program = "/bin/bash -c 'cd /home/sandip/current && /usr/bin/ahnctl start adhearsion'"
    if totalmem > 100.0 MB for 3 cycles then restart
    if cpu usage > 95% for 3 cycles then restart
```

#### Nginx monit configuration
```
check process nginx with pidfile /opt/nginx/logs/nginx.pid
    start program = "/opt/nginx/sbin/nginx"
    stop  program = "/opt/nginx/sbin/nginx -s stop"
    if cpu is greater than 70% for 3 cycles then alert
    if cpu > 80% for 5 cycles then restart
    if 10 restarts within 10 cycles then timeout
```
