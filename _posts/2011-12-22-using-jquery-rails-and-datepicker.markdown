---
layout: post
title: "using jQuery-rails and datepicker, timepicker & datetimepicker"
date: 2011-12-22T03:52:00+05:30
comments: false
categories:
 - rails 3
 - datepicker
 - jQuery
---

jQuery datepicker plugin is used to display inline calendar popup that eases user input experience while entering date/time fields. Calendar can be easily binded to any html DOM element. To Apply different styles download css from [here](http://jqueryui.com/demos/datepicker)

```
# Gemfile
gem "jquery-rails"
```

Install Bundle
```
bundle install
```

Add inside `application.js.coffee`
```coffeescript
# app/assets/javascripts/application.js.coffee 
//= require jquery
//= require jquery_ujs
//= require jquery-ui
```

Access in layout
```haml
# app/views/layouts/application.html.haml
= stylesheet_link_tag "application"
= stylesheet_link_tag "http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.16/themes/base/jquery-ui.css"
= javascript_include_tag "application"
```

Before using timepicker download jquery-ui-timepicker js 
```
wget http://trentrichardson.com/examples/timepicker/js/jquery-ui-timepicker-addon.js app/assets/javascripts/
```

Add timepcker js to `application.js.coffee`
```coffeescript
//= require jquery-ui-timepicker-addon
```

View form
```haml
## inside views
# app/views/users/_form.html.haml
= form_for @user ||= User.new do |f|
  = f.text_field :birth_date, :id => 'birthDate'
  = f.text_field :birth_time, :id => 'birthTime'
  = f.text_field :exam_on, :id => 'examOn'
:javascript
  $(document).ready(function() {
    $("#birthDate").datepicker();
    $("#birthTime").timepicker();
    $("#examOn").datetimepicker();
    $('#ui-datepicker-div').removeClass('ui-helper-hidden-accessible')
```

Customizations
```javascript
$('#birth_date').datepicker({ dateFormat: 'dd-mm-yy' });
$('#birth_date').datepicker({ disabled: true });
$("#examOn").datetimepicker({ ampm: true });
$("#examOn").datetimepicker({ timeFormat: 'h:m', separator: ' @ ' });
```
