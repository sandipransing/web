---
layout: post
title: "Print particular DIV inside HTML Page"
date: 2011-09-12T13:26:00+05:30
comments: false
categories:
 - general
---

Hiding Whole page content and displaying only particular div using css

```css
@media print
  {
    body * { visibility: hidden; }
    #PrintDiv * { visibility: visible; }
    #PrintDiv { position: absolute; top: 40px; left: 30px; }
  }
```
