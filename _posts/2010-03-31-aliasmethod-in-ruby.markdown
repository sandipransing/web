---
layout: post
title: "alias methods in ruby"
date: 2010-03-31T17:46:00+05:30
comments: false
categories:
 - ruby
 - meta-programming
---

## Alias method in ruby

Ruby classes provides a alias_method that can be used to reuse the existing methods.
Consider a situation where you need different methods which has same code and ONLY they have different names.

In this case alias_method uses suits best choice instead duplicating same code or writing common method that will get used in all methods, as i did before. Example, I have methods search, home, index all are doing same functionality.

## Old approach

```ruby
# URL / 
def index 
 list 
end 
# URL /search
def search 
 list 
end 

#URL /home 
def home 
 list 
end 
private 
def list 
# code here 
end
```
 
## Correct approach in ruby
```
def index 
 # code here 
end 
alias_method :home, :index 
alias_method :search, :index 
```
## Attributes aliasing in ruby

Same way one can easily rename existing class attribute names using alias_attribute method.
## alias_attribute(new_name, old_name)

```
alias_attrinute :username, :login
```
## More practical use

while deprecating attributes, methods in gems, plugins, extensions, libraries always use aliases in order to maintain backward compatibility.

Got easy ?? that's where ruby rocks !
