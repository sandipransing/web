---
layout: post
title: "vi / vim Shortcuts"
date: 2009-05-22T18:23:00+05:30
comments: false
categories:
 - vim
 - rails
 - ubuntu
---

As i am newb on linux macine, i dont know vi shortcuts.
so, i am listing down shortcuts which i am getting familier :)

```
# Open file
vi filename

# Insert in file
i

# Exit file
q

# Forced exit
q!

# Save file
wq

# Forced save
wq!

# Copy no of lines
yy

# Paste copied lines
pp

# Undo changes
uu

# Delete lines
dd

# To search and replace string in vi
:%s/search_string/replacement_string/g

# To find particular word in file
?string1
```
If you have any quick list, please let me know !

Below post is reblogged from http://nakuls77.wordpress.com/2008/09/14/using-vi-editor/#comment-144

> Vi is a very powerful editor that has been part of nearly every linux/unix distro for a very long time. Many people stray away from Vi due to the curve involved in learning this editor. This guide will help you dive into the world of Vi. Once you do, you will understand why many prefer Vi over other editors.

There are countless options for Vi that can be set up inside the /etc/vimrc file.

Auto-indent and creating a macro to change the color of the background.
```
# vi /etc/vimrc
```
## Auto-Indent Option -

This option is so when you tab-space out a line and then press enter, the cursor will move to the next line, automatically indented to the start of the previous line. This helps programmers indent out code easily to create clean looking code. To implement this feature, please add the following line to the bottom of your /etc/vimrc file:
```
set ai
```
Please note that generally speaking only programmers will want to turn ON auto-indent.

## Background Color -

Some SSH clients have white backgrounds and some have black backgrounds. This can make it very difficult to see the screen depending on the color of the text. This macro allows you to configure F11 to change the background color & color of the text. To implement this feature, please add the following lines to the bottom of your /etc/vimrc file:

set background=dark
map :let &background = ( &background == “dark”? “light” : “dark” )

I would highly recommend adding in the Background Color option. However if you do not wish to implement either of these features, then please proceed on in the guide.

Commands

## Cursor Movement
0 (Zero) Start of Line
^ – First non-Blank Character of Line
$ – End of Line
G Go To Command (prefix with number 5G goes to line 5)

Insert Mode Inserting/Appending Text
Insert Start Insert Mode at Cursor
A Append at the end of the Line
o open (append) blank link below current line
I Open blank line above current line
Esc Exit Insert Mode

## Editing

r Replace a single character (does not use insert mode)
J Join line below with the current one
cc Change (replace) an entire line
cw Change (replace) to the end of word
c$ – Change (replace) to the end of line
s Delete character at cursor and substitute text
S Delete line at cursor and substitute text (same as cc)
u Undo
. – Repeat last command

## Cut and Paste

yy Copy(yank) a line
2yy Copy(yank) two lines
yw Copy(yank) word
p Paste the clipboard after the cursor
P Paste the clipboard before the cursor
dd Delete a line
dw Delete the current word
x Delete the current character

## Exiting

:w Write (save) the file, but don’t exit
:wq Write & Quit
:q Quit
:q! – Quit and don’t save changes

## Searching

/pattern Search for a pattern
?pattern Search backward for a pattern
n Repeat last search in same direction
N Repeat last search in opposite direction
:%2/old/new/g Replace all old with new throughout file
:%s/old/new/gc Replace all old with new throughout file with confirmations

## Working with Multiple Files

:e filename Edit a file in a new buffer
:bn Go to next buffer
:bp Go to previous buffer
:bd Close file(buffer)
:sp filename Open a file in a new buffer and split window
ctrl-ws Split windows
ctrl-ww Switch between windows
ctrl-wq Quite a window
ctrl-wv Split windows vertically
ctrl-wq Quite a window
ctrl-wv Split windows vertically
