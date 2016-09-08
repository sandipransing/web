---
layout: post
title: "Vim editor for ruby on rails development using rails.vim"
date: 2010-02-19T20:20:00+05:30
comments: false
categories:
 - vim
 - rails
---

#### Vim install On CentOS
```
yum install vim-enhanced
```

#### Vim install on Ubuntu machine

Install vim-full using command
```
apt-get install vim
```

While coding with ruby, html, erb, haml, js and stylesheets.
It is great pain to indent code. Using rails vim one can easily keep code always indented.

This increases code readability and minimizes effort, bugs and finally proves ease of using vim editor.

`rails.vim` script contains lot of syntax highlighter and indentation plugins that really helps development needs.

If you have git installed then clone it under .vim directory of your profile
```
git clone git://github.com/sandipransing/rails_vim.git ~/.vim
```

#### To install from zip file download and extract it inside `~/.vim` directory

Command to [download](http://www.vim.org/scripts/download_script.php?src_id=11920) from console
```
wget http://www.vim.org/scripts/download_script.php?src_id=11920 /rails.vim 
```
This is how my editor looks like

<a href="http://1.bp.blogspot.com/_WmOmsgqsanw/S5voyvHVstI/AAAAAAAAAJI/bi-mSfyN5no/s1600-h/IDE.png" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="215" src="http://1.bp.blogspot.com/_WmOmsgqsanw/S5voyvHVstI/AAAAAAAAAJI/bi-mSfyN5no/s400/IDE.png" width="500" /></a>

Open your ~/.bashrc and at the bottom add:

```
alias vi=vim
export EDITOR=vim
```
