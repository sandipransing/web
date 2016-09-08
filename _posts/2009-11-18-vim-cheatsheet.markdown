---
layout: post
title: "VI/ VIM cheats"
date: 2009-11-18T01:20:00+05:30
comments: false
categories:
 - vim
---

## Movements
```    
b   previous word
w   next word
e        end of word
0/^      begining of line
$        end of line
G        end of file
1G/gg    begining of file
/pattern  search next
?pattern  search previous
n         repeat    search forword ( i.e next occurence )
N         repeat    search backword
:line     goto line specified
```
## Modes
```
i   insert mode
r   replace mode
s   delete character under cursor and eneter insert mode
```
## Delete
```
x    delete character under cursor
dd   delete current line
line dd delete number of lines specified
```

## Copy
```
yy        copy current line
pp        print copied contents
line yy   copy number of lines specified
```
## Visual
```
v     enter visual mode
aw    highlight word
as    highlight sentence
ap    highlight paragraph
ab    highlight block
```

## Undo/ Redo
```
u       undo
cntrl+r redo
```

## autocomplete
```
cntrl+x   enter completion mode
cntrl+p   display autocomplete options
```

## Uppercase/ Lowercase
```
guu   lowercase line
gUU   uppercase line
```
## Regx replace
```
range s/foo/bar/arg - replace foo with bar in 'range' with

Values of 'range':
%               whole file
number          that particular line
none            apply to current line only

values of 'arg':
none  apply to first occurrence
g     global (all occurrences)
```

## Select/Macros
```
qchar      start recording macro storing it in register 'char'
q          end recording
@char      replay the macro stored in 'char'
:1,10 norm! @char run the macro stored in 'char over the 1-10 line range
```
