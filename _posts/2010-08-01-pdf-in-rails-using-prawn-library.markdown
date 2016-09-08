---
layout: post
title: "PDF in rails using prawn library"
date: 2010-08-01T16:45:00+05:30
comments: false
categories:
 - ruby
 - prawn
 - rails
---

Building PDF Document in ruby & rails application using prawn Library
**Brief**. Before getting started with this tutorial, I would like to thanks **Greg and 
Prawn team** for their awesome work towards ruby and rails community

## Installing prawn (core, layout, format, security) 
```
gem install prawn
```
or
Add following line in rails environment file inside initializer block
```
config.gem 'prawn'
```
Optionally you can specify version to be used and then run task
```
rake gems:install
```
Generating pdf using rails console

```
ruby script/console
pdf = Prawn::Document.new
```
It creates new pdf document object. Here you can additionally pass options parameters such as -

```ruby
Prawn::Document.new(:page_size   => [11.32, 8.49], :page_layout => :portrait)
Prawn::Document.new(A0) Here A0 is page size.
Prawn::Document.new(:page_layout => :portrait,
                     :left_margin => 10.mm,    # different
                     :right_margin => 1.cm,    # units
                     :top_margin => 0.1.dm,    # work
                     :bottom_margin => 0.01.m, # well
                     :page_size => 'A4')

pdf.text("Prawn Rocks")
=> 12
 pdf.render_file('prawn.pdf')
=> #<file:prawn.pdf (closed)=""></file:prawn.pdf>
```
Here is output file generated
[click](http://github.com/sandipransing/prawn-example/raw/master/sample/prawn.pdf)

Now let's go through other goodness of prawn library
```
pdf  = Prawn::Document.new('A3') do
```
## FONTS
[click](http://github.com/sandipransing/prawn-example/raw/master/sample/font.pdf)
```
# Specify font to be used or specify path to font file.
font "times.ttf"
font("/times.ttf")
```
## TEXT
[click](http://github.com/sandipransing/prawn-example/raw/master/sample/font.pdf)

```
text 'Sandip Ransing', :size => 41, :position => :center, :style => :bold
```
## STROKE LINE 
[click](http://github.com/sandipransing/prawn-example/raw/master/sample/stroke.pdf)
```
stroke do
      rectangle [300,300], 100, 200
 end
```
## IMAGE
[click](http://github.com/sandipransing/prawn-example/raw/master/sample/image.pdf)

Displaying Local file system Image
```
image 'sandip.png', :height => 50, :position => :center, :border => 2
Scale Image

image 'sandip.png', :scale => 0.5, :position => :left
Display Remote image from Internet inside pdf

require "open-uri"
 image open('http://t2.gstatic.com/images?q=tbn:kTG6gAKrnou2gM:http://www.facebook.com/profile/pic.php?uid=AAAAAQAQrLXvTWfyY2ANjttV8D1c0QAAAAnDHPFJe0pPFR84iIzXPKro&t=1")
end
```
## LINE BREAKS

```
movedown(20)

```
## TABLE/GRID
[click](http://github.com/sandipransing/prawn-example/raw/master/sample/table.pdf)
```
data = [
    ["Name", {:text => 'Sandip Ransing', :font_style => :bold, :colspan => 4 }],
    ["Address", {:text => 'SHIVAJINAGAR, PUNE 411005', :colspan => 4 }],
    ["Landmark",{:text => 'NEAR FC COLLEGE', :colspan => 4 }],
    ["Mobile","9860648108", {:text => "", :colspan => 3 }],
    ["Education", {:text => "Bachelor in Computer Engineering", :colspan => 4 }],
      ["Vehicle", 'Hero Honda',"Reg. No.", {:text => "MH 12 EN 921", :colspan => 3 }],
      ["Additional", "GDCA", "class", 'First', ""],
      [{:text => "Areas of Speciality", :font_style => :bold}, {:text => "Ruby, Rails, Radiant, Asterisk, Adhearsion, Geokit, Prawn, ....,...", :font_style => :bold, :colspan => 4}],
      [{:text => "Website", :colspan => 2},{:text => "www.funonrails.com", :colspan => 3}],
      [{:text => "Company", :colspan => 2},{:text => "Josh Software", :colspan => 3}]
  ]
table data, 
    :border_style => :grid, #:underline_header
    :font_size  => 10, 
    :horizontal_padding => 6,
    :vertical_padding   => 3,
    :border_width => 0.7, 
    :column_widths => { 0 => 130, 1 => 100, 2 => 100, 3 => 100, 4 => 80 }, 
    :position => :left,
    :align => { 0 => :left, 1 => :right, 2 => :left, 3 => :right, 4 => :right }
```
## LINKS
[click](http://github.com/sandipransing/prawn-example/raw/master/sample/link.pdf)
```
link_annotation([200, 200, 500, 40],:Border => [0,0,1], :A => { :Type => :Action, :S => :URI, :URI => Prawn::LiteralString.new("http://twitter.com/sandipransing") } )
link_annotation(([0, 100, 100, 150]), :Border => [0,0,1], :Dest => s"http://funonrails.com")
```
## PDF Security
[click](http://github.com/sandipransing/prawn-example/raw/master/sample/secure.pdf)
```
encrypt_document :user_password => 'hello', :owner_password => 'railer',
    :permissions => { :print_document => false }
```
## Prawn Inline Formatting

Prawn-format supports inline text formatting that gives user enough flexibility to use html tags

```
require 'prawn/format'
text 'This is Strong  text', :inline_format => true
text 'This is bold text \n It should be on newline.', :inline_format => true
```
## SAVE Generated PDF
```
end
pdf.render_file 'my.pdf'
```
!!! NOTE: As of time now 'prawn-format' is incompatible with latest prawn gem, It is compatible with prawn version <= 0.6 s
