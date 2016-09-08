---
layout: post
title: "number to indian currency helper for rails with WebRupee"
date: 2011-03-17T02:13:00+05:30
comments: false
categories:
 - rails
 - views
---

> rails has built in `number_to_currency` helper which takes options like unit, delimeter, seperator which displays foreign currency correctly but somehow it is not best suited for indian currency.

Below is how we managed 2 years ago to display indian currency formatted properly with comma as seperator. personally i think it could be more better than what it is currently ;)

## Number to indian currency(rupees) helper
```ruby
module ApplicationHelper
  def number_to_indian_currency(number)
    if number
      string = number.to_s.split('.')
      number = string[0].gsub(/(\d+)(\d{3})$/){ p = $2;"#{$1.reverse.gsub(/(\d{2})/,'\1,').reverse},#{p}"}
      number = number.gsub(/^,/, '') + '.' + string[1] if string[1]
      # remove leading comma
      number = number[1..-1] if number[0] == 44
    end
    "Rs.#{number}"
  end
```

## Sample Output for different combinations
```ruby
>> helper.number_to_indian_currency(2000)
=> "Rs.2,000"
>> helper.number_to_indian_currency(2040)
=> "Rs.2,040"
>> helper.number_to_indian_currency(2040.50)
=> "Rs.2,040.5"
>> helper.number_to_indian_currency(2040.54)
=> "Rs.2,040.54"
>> helper.number_to_indian_currency(1222040.54)
=> "Rs.12,22,040.54"
```
After doing google today found from Piyush Ranjan's Blog that yes there are ways to optimize code.
## Optimized Version
```ruby
module ApplicationHelper
  def number_to_indian_currency(number)
    "Rs.#{number.to_s.gsub(/(\d+?)(?=(\d\d)+(\d)(?!\d))(\.\d+)?/, "\\1,")}"
  end
end
```
Waw one line of code, Look at the beauty of regular expression :) Truely amazing !
## Integrating Webrupee symbol 
First include follwing stylesheet in your layout
```css
//public/stylesheets/font.css 
@font-face {
  font-family: "WebRupee";
  font-style: normal;
  font-weight: normal;
  src: local("WebRupee"), url("http://cdn.webrupee.com/WebRupee.V2.0.ttf") format("truetype"), url("http://cdn.webrupee.com/WebRupee.V2.0.woff") format("woff"), url("http://cdn.webrupee.com/WebRupee.V2.0.svg") format("svg");
}
.WebRupee {
  font-family: 'WebRupee';
}
```
## Improved Version of Helper
```ruby
module ApplicationHelper
  def number_to_indian_currency(number, html=true)
    txt = html ? content_tag(:span, 'Rs.', :class => :WebRupee) : 'Rs.'
    "#{txt} #{number.to_s.gsub(/(\d+?)(?=(\d\d)+(\d)(?!\d))(\.\d+)?/, "\\1,")}"
  end
end
```

## Usage
```ruby
>> helper.number_to_indian_currency(400)
=> "<span class="WebRupee">Rs.</span> 400"
>> helper.number_to_indian_currency(5921, false)
=> "Rs. 5,921"
>> helper.number_to_indian_currency(9921)
=> "<span class="WebRupee">Rs.</span> 9,921"
```

This will show you rupees symbol on your webpages.
