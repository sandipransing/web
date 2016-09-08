---
layout: post
title: "Fix for ruby 1.8.7 incompatibity"
date: 2009-05-13T19:22:00+05:30
comments: false
categories:
 - ruby
---
Last week i shifted from `ruby` version `1.8.6` to `1.8.7` :-)
But over a week i found that rails `1.2.3` applications developed in ruby `1.8.6` are not working in ruby `1.8.7`

From lot of search on google, i found that its issue with ruby `1.8.7`

So in order to fix this bug, i started searching `fix / solution` for this incompatibity and got below fix.

You just need to fix file `delegate.rb` and done

## Diff of `/branches/ruby_1_8_7/lib/delegate.rb`

```
--- branches/ruby_1_8_7/lib/delegate.rb 2008/05/31 15:17:53 16732
+++ branches/ruby_1_8_7/lib/delegate.rb 2008/06/02 10:52:07 16756
@@ -163,9 +163,9 @@
   # Checks for a method provided by this the delegate object by fowarding the 

   # call through \_\_getobj\_\_.
   # 
-  def respond_to?(m)
+  def respond_to?(m, include_private = false)
     return true if super
-    return self.__getobj__.respond_to?(m)
+    return self.__getobj__.respond_to?(m, include_private)

   end

   #
@@ -270,9 +270,9 @@
       end
       @_dc_obj.__send__(m, *args)
     end
-    def respond_to?(m)  # :nodoc:
+    def respond_to?(m, include_private = false)  # :nodoc:
       return true if super

-      return @_dc_obj.respond_to?(m)
+      return @_dc_obj.respond_to?(m, include_private)
     end
     def __getobj__  # :nodoc:
       @_dc_obj
```
You have to make necessary changes and you are ready to work with ruby 1.8.7.
need not downgrade ruby version :)

## for Hash error

Add following line in `config/environment.rb`
```ruby
unless '1.9'.respond_to?(:force_encoding)
  String.class_eval do
    begin
      remove_method :chars
    rescue NameError
      # OK
    end
  end
end
```
