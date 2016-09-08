---
layout: post
title: "Ghost-Type input Placeholder for search text fields using jQuery"
date: 2011-04-07T14:14:00+05:30
comments: false
categories:
 - jQuery
---

Placeholder Demo:
<input type=text placeholder='Enter search text here'/>
Click on input box and placeholder text will disappear.

Ghost-type Demo: 
<input id='home-search' class="query" placeholder='Enter search text here' type="search"/>
After page load observe inside input box to enter placeholder text char by char.
 
jQuery placeholder plugin can be used to display default text inside input text fields. HTML5 introduced type search fields with placeholder attribute support.
```
<input class="query" id="home-search" placeholder="Enter search text here" type="search" />
```

It should display default text "Enter search text here" inside text-field and when user tries to type something it disappears.
In order to achieve text marquee like effect to default text value with jQery use following jQuery code. It will type text character by character from left to right.

Same effect can be achieved by creating gif image of typing and placing it as background-image to text field with certain drawbacks like changing default text on the fly.

Place below code inside javascript tag in your html file
```javascript
#jQuery Code
$(document).ready(function(){
  var search = "#home-search";
  var chars = $(search).attr('placeholder').split('');
  $.each(chars, function(i, v){
    setTimeout(function() {
      $(search).val((chars.slice(0, i+1).join('')));
    }, 100*i);
  });
});
```
<script type='text/javascript'>
$(document).ready(function(){
  var search = "#home-search";
  var chars = $(search).attr('placeholder').split('');
  $.each(chars, function(i, v){
    setTimeout(function() {
      $(search).val((chars.slice(0, i+1).join('')));
    }, 1000*i);
  });
});
</script>
Got easy ??
