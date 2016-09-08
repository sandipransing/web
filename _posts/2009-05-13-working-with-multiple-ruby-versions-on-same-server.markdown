---
layout: post
title: "Working with multiple ruby versions on same server"
date: 2009-05-13T20:12:00+05:30
comments: false
categories:
 - ruby
---

This post is reblogged from Michael Greenly blog [post](http://blog.michaelgreenly.com/2008/08/multiple-versions-of-ruby-on-ubuntu-2.html)

I recently changed how I'm handling multiple simultaneous Ruby installations and I'd like to share.

What I needed was to make it convenient to switch between the system provided packages and specific, from source, installations. After some experiments I decided to use 'update-alternatives' to do it.

Here's a quick walk through...

First I removed all of my Ruby and RubyGem environment variables, they're not needed.

Then I installed Ubuntu's default Ruby packages via apt-get.
```
$ sudo apt-get install ruby irb ri rdoc libruby-extras rubygems ruby1.8-dev
```
Next I downloaded and installed alternate versions of Ruby from source. In this example I'm going to use two additional versions; the newest stable release and the newest development release.
```
$ cd /tmp
$ wget -c ftp://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.7-p71.tar.gz
$ tar -xvzf ruby-1.8.7-p71.tar.gz
$ cd ruby-1.8.7-p71
$ ./configure --prefix=/opt/ruby-1.8.7-p71
$ make
$ sudo make install
$ wget -c ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.0-3.tar.gz
$ tar -xvzf ruby-1.9.0-3.tar.gz
$ ./configure --prefix=/opt/ruby-ruby-1.9.0-3
$ make
$ sudo make install
```
At this point I have three versions of Ruby installed and each can be accessed through it's full path.
```
$ /usr/bin/ruby --version
# ruby 1.8.6 (2007-09-24 patchlevel 111) [i486-linux]
$ /opt/ruby-1.8.7-p71/bin/ruby --version
# ruby 1.8.7 (2008-08-08 patchlevel 71) [i686-linux]
$ /opt/ruby-1.9.0-r18217/bin/ruby --version
# ruby 1.9.0 (2008-07-25 revision 18217) [i686-linux]
```
You'll also notice that the default installation is the one provided by Ubuntu.
```
$ ruby --version
# ruby 1.8.6 (2007-09-24 patchlevel 111) [i486-linux]
```
Next we'll use 'update-alternatives' to make it a bit easier to switch between them. You could do this on the command line but it becomes a fairly long nasty command so I found it easier to write a quick shell script and run it. The script:
```
update-alternatives --install \
/usr/local/bin/ruby ruby /usr/bin/ruby 100 \
--slave /usr/local/bin/erb erb /usr/bin/erb \
--slave /usr/local/bin/gem gem /usr/bin/gem \
--slave /usr/local/bin/irb irb /usr/bin/irb \
--slave /usr/local/bin/rdoc rdoc /usr/bin/rdoc \
--slave /usr/local/bin/ri ri /usr/bin/ri \
--slave /usr/local/bin/testrb testrb /usr/bin/testrb

update-alternatives --install \
/usr/local/bin/ruby   ruby /opt/ruby-1.8.7-p71/bin/ruby 50 \
--slave /usr/local/bin/erb erb /opt/ruby-1.8.7-p71/bin/erb \
--slave /usr/local/bin/gem gem /opt/ruby-1.8.7-p71/bin/gem \
--slave /usr/local/bin/irb irb /opt/ruby-1.8.7-p71/bin/irb \
--slave /usr/local/bin/rdoc rdoc /opt/ruby-1.8.7-p71/bin/rdoc \
--slave /usr/local/bin/ri ri /opt/ruby-1.8.7-p71/bin/ri \
--slave /usr/local/bin/testrb testrb /opt/ruby-1.8.7-p71/bin/testrb

update-alternatives --install \
/usr/local/bin/ruby   ruby /opt/ruby-1.9.0-r18217/bin/ruby 25 \
--slave /usr/local/bin/erb erb /opt/ruby-1.9.0-r18217/bin/erb \
--slave /usr/local/bin/gem gem /opt/ruby-1.9.0-r18217/bin/gem \
--slave /usr/local/bin/irb irb /opt/ruby-1.9.0-r18217//bin/irb \
--slave /usr/local/bin/rdoc rdoc /opt/ruby-1.9.0-r18217/bin/rdoc \
--slave /usr/local/bin/ri ri /opt/ruby-1.9.0-r18217/bin/ri \
--slave /usr/local/bin/testrb testrb /opt/ruby-1.9.0-r18217/bin/testrb
```
What that does is create a group of applications under the generic name Ruby. In addition each application has several slave applications tied to it; erb, irb, etc... In defining each application we specify what symbolic link it will be accessed through and where the application is actually installed. In my case Ubuntu installed Ruby in /usr/bin and the source installed versions are in /opt. All of the installations will be accessed through the generic name Ruby and will have there symbolic links created in /usr/local/bin. I choose /usr/local/bin because it supercedes /usr/bin in the default path.

Before moving on make sure that 'update-alternatives' sees all of our Ruby installations:
```
$ update-alternatives --list ruby
# /opt/ruby-1.9.0-r18217/bin/ruby
# /opt/ruby-1.8.7-p71/bin/ruby
# /usr/bin/ruby
```
Now switching between them is as easy as running the 'update-alternatives' command and selecting the number of the installation you'd like to use. Example:
```
$ sudo update-alternatives --config ruby
```
It's important to keep in mind that each installation is separate. So for example if you install RubyGems while using /usr/bin/ruby it will not be available to /opt/ruby-1.9.0-r18217/bin/ruby, or /opt/ruby-1.8.7-p71/bin/ruby, etc.... 

While it's probably possible to use a shared repository for RubyGems across multiple installations I haven't tried it and instead have choosen to use multiple separate RubyGem installs, one for each Ruby installation.

Also RubyGem's bindir will most likely not be in your path. To get around this I created a short script called 'gemexec' in /usr/local/bin
```
#!/usr/bin/env ruby

require 'rubygems'

if ARGV.size > 1
exec "#{Gem.bindir}/#{ARGV.shift}",ARGV.join(" ")
else
exec "#{Gem.bindir}/#{ARGV.shift}"
end
```
This script uses the RubyGems installation of the currently selected Ruby to determine where the executable gem should be found, then runs it with any additional command line arguments provided. example:
```
$ gemexec rake --version
# rake, version 0.8.1
```
With all that in place the only thing to watch out for is other peoples scripts that hardcode the shebang line with something like "#!/usr/bin/ruby". What I do myself, and prefer in general, is to use "#!/usr/bin/env ruby".
