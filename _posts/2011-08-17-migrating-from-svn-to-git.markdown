---
layout: post
title: "Migrating from svn to git"
date: 2011-08-17T19:44:00+05:30
comments: false
categories:
 - git
 - svn
---

Migrating from svn to git 

**1. Install git-svn**
```
sudo apt-get install git-svn
```

**2. Clone svn repositoy**
```
git svn clone -s svn://server/project_myt/trunk proj
```

To add svn users and git users mapping
Create author-file
```
# authorsfile 
sandip = sandipransing 
amit = amitk 
root = sandipransing 
```

```
git svn --author-file=authorfile clone -s svn://server/project_myt/trunk proj
```

**3. Add git repository ur**
```
cd proj
git remote add git@github.com:USERNAME/project.git
```

**4. Push repository to git**
```
git push origin master
```

To do svn pull for latest changes
```
git svn rebase
```
