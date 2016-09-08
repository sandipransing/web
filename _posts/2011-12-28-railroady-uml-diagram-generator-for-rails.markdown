---
layout: post
title: "railroady UML diagram generator for rails"
date: 2011-12-28T23:42:00+05:30
comments: false
categories:
 - railroady
 - rails 3
---

railroady is UML class diagram generator for rails.
First you need to install `graphviz` pkg in order to have `dot` , `neato` commands available

```
# Gemfile
group :development, :test do
  gem railroady
end
```

Run below command to generate MVC diagrams
```
bundle install
rake diagram:all
```

Individual diagram generation

**1.  Model Diagram**
```
railroady -M | dot -Tpng > models.png
```

**2.  Controller Diagram**
```
railroady -C | dot -Tpng > controllers.png
```

**3.  AASM Diagram**
```
railroady -A | dot -Tpng > aasm.png
```

Commands
```
-M, --models                     Generate models diagram
-C, --controllers                Generate controllers diagram
-A, --aasm                       Generate "acts as state machine" diagram
```

Other options which can be passed along-with commands -
```
# Common options
-b, --brief                      Generate compact diagram
                                   (no attributes nor methods)
-s, --specify file1[,fileN]      Specify given files only for the diagram
                                   (can take a glob pattern)
-e, --exclude file1[,fileN]      Exclude given files
                                   (can take a glob pattern)
-i, --inheritance                Include inheritance relations
-l, --label                      Add a label with diagram information
                                   (type, date, migration, version)
-o, --output FILE                Write diagram to file FILE
-v, --verbose                    Enable verbose output
                                   (produce messages to STDOUT)
```
Models diagram options:
```

-a, --all                        Include all models
                                   (not only ActiveRecord::Base derived)
    --all-columns                Show all columns 
                                   (not just content columns)
    --hide-magic                 Hide magic field names
    --hide-types                 Hide attributes type
-j, --join                       Concentrate edges
-m, --modules                    Include modules
-p, --plugins-models             Include plugins models
-t, --transitive                 Include transitive associations
                                 (through inheritance)
```

Controllers diagram options:
```
--hide-public                Hide public methods
--hide-protected             Hide protected methods
--hide-private               Hide private methods
```
Other Options
```
-h, --help                       Show this message
    --version                    Show version and copyright
```
