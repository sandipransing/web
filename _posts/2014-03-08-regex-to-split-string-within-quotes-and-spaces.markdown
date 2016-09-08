---
layout: post
title: "regex to split string within quotes and spaces"
date: 2014-03-08 21:32:37 +0530
comments: false
published: false
categories: jquery
---

I wanted to split a string based on white spaces except when if anything is enclosed inside quotes then i don't want to split that piece of string from getting splitted.

It means string will be splitted based on consecutive double quotes from left to right and remaining string will be spitted on spaces.
<!--more-->
#### Example: 
Splitting of below string 
```
"html css java 'ruby on rails' python jquery 1's" 
```
should give array of below resulting strings 
```
html
css
java
ruby on rails
python
jquery
1's
```
#### Regular expression
Below regular expression gives array matched string seperated by quotes and then spaces 
```
/'.*?'|".*?"|\S+/g
```

```javascript
tokens = "html css java 'ruby on rails' python jquery 1's".match(/'.*?'|".*?"|\S+/g);
console.log(tokens);
```
#### Result
```
0: "html"
1: "css"
2: "java"
3: "'ruby on rails'"
4: "python"
5: "jquery"
6: "1's"
length: 7
```
#### Demo
<input type='text' data-id='split' placeholde='Say something' />

<div data-id='result'>
</div>
<script type='text/javascript'>
  $("[data-id=split]").keyup(function(){
    query = $(this).val();
    if(query.length) {
      tokens = query.match(/'.*?'|".*?"|\S+/g);
      result = $.map(tokens, function(t, i){ return(i+1+": "+t); }).join('<br/>');
      $("[data-id=result]").html("<b>Result:</b><br/>").append(result)
    }
  })
</script>
