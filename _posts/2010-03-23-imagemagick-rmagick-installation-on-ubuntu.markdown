---
layout: post
title: "Imagemagick/ RMagick Installation on ubuntu"
date: 2010-03-23T18:11:00+05:30
comments: false
categories:
 - installation
 - ubuntu
 - imagemagick
 - rmagick
---

#### ImageMagick Installation

Ubuntu machine has default apt-get package manager.
To install imagemagick following packages needs to be installed.

```
sudo apt-get install imagemagick librmagick-ruby libmagickwand-dev
```

#### RMagick gem install
```
gem install rmagick
```
If you still facing problems with gem installation, Please look that following packages are installed
on your system.

```
dpkg -l | grep libmagickcore-dev graphicsmagick-libmagick-dev-compat
```

If there are no packages installed then try to install then first.
```
sudo apt-get install libmagickcore-dev graphicsmagick-libmagick-dev-compat
```

Try to install rmagick gem again. Now it should be installed without any error.
