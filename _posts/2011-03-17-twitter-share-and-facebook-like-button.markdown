---
layout: post
title: "Twitter share and facebook like button for haml-rails, html/erb"
date: 2011-03-17T02:45:00+05:30
comments: false
categories:
 - social
 - general
---

Twitter share and facebook like button for `html/erb`
```html
<div class='spread'>
  <div class='twshare left'>
    <a href="http://twitter.com/share" class="twitter-share-button" data-count="horizontal" data-via="funonrails">Tweet</a><script type="text/javascript" src="http://platform.twitter.com/widgets.js"></script>
  </div>
  <script src="http://connect.facebook.net/en_US/all.js#xfbml=1"></script><fb:like href="" layout="button_count" show_faces="false" width="450" font=""></fb:like>
</div>
```

Twitter share and facebook like button for `haml`
```haml
.spread
  .twshare.left
    %a.twitter-share-button.left{"data-count" => "horizontal", "data-via" => "fuonrails", :href => "http://twitter.com/share"} Tweet
    %script{:src => "http://platform.twitter.com/widgets.js", :type => "text/javascript"}
  .fshare.left{:style => 'padding-left: 0px; margin-left: 10px;'}
    %script{:src => "http://connect.facebook.net/en_US/all.js#xfbml=1"}
    %fb:like{:layout => "button_count", :show_faces => "false", :width => "450"}
  .CLR
```

See example Buttons listed below:
<div class='spread'>
  <div class='twshare left'>
    <a href="http://twitter.com/share" class="twitter-share-button" data-count="horizontal" data-via="funonrails">Tweet</a><script type="text/javascript" src="http://platform.twitter.com/widgets.js"></script>
  </div>
  <script src="http://connect.facebook.net/en_US/all.js#xfbml=1"></script><fb:like href="" layout="button_count" show_faces="false" width="450" font=""></fb:like>
</div>
