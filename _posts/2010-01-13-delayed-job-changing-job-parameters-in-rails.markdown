---
layout: post
title: "Delayed Job changing job parameters in Rails"
date: 2010-01-13T19:59:00+05:30
comments: false
categories:
 - delayed-job
 - rails
---
Delayed job variables initialization written on at collectiveidea / delayed_job doesn't work.
```ruby
# config/initializers/delayed_job_config.rb
Delayed::Worker.destroy_failed_jobs = false
Delayed::Worker.sleep_delay = 60
Delayed::Worker.max_attempts = 3
Delayed::Worker.max_run_time = 5.minutes
```
Here is the correct way of doing it as mentioned on tobi / delayed_job 
```ruby
# config/initializers/delayed_job_config.rb
Delayed::Job.destroy_failed_jobs = false
silence_warnings do
  Delayed::Job.const_set("MAX_ATTEMPTS", 3)
  Delayed::Job.const_set("MAX_RUN_TIME", 5.minutes)
end
```
