---
layout: post
title: "Rails Coding Standards"
date: 2009-05-13T16:48:00+05:30
comments: false
categories:
 - best-practices
 - rails
---

> IN ESSENCE: ALL CODE SHOULD BE READABLE!

1.  DO NOT OPTIMISE for performance - OPTIMISE FOR CLARITY OF CODE
2.  STYLE: use 2 spaces for indent (not tabs)
3.  STYLE: Line up hash arrows for readability
4.  STYLE: put spaces around => hash arrows
5.  STYLE: put spaces after ‘,’ in method params - but none between method names and ‘(’
6.  VIEWS: use HAML for views
7.  VIEWS: break up the structure with white space to help readability - VERTICALLY TOO!
8.  VIEWS STYLE: Rely on structure of page, without having to insert messages or new components…
9.  LOGIC: Rails Models should be as heavy as in logic and controllers should be lightweight as much as .
10. Example: Effect to visually highlight then drop out an existing element rather than flash a message
11. Example: Highlight newly added row rather than a message about it
12. AVOID logic in views - they should be simple
13. Views indentation should be well formatted and not like this :
#### WRONG
```html
<% for joke in @jokes %>
<div class="joke">
<p>
<%= h(truncate(joke.joketext, 20)) %>
<%= link_to 'Read this joke', {:action => 'show_joke', :id => joke} %>
</p>
<p class="author>
by <%= h(joke.author.full_name) %></p>
</div>
<% end %>
```
#### CORRECT
```html
<% for joke in @jokes %>
  <div class="joke">
    <p>
      <%= h(truncate(joke.joketext, 20)) %>
      <%= link_to 'Read this joke', { :action => 'show_joke', :id => joke } %>
    </p>
    <p class="author>
      by <%= h(joke.author.full_name) %>
    </p>
  </div>
<% end %>
```
14. Put html generating logic into helpers
15. instead of inline ruby logic, add to models (filtering, checking)
16. NEVER use ActiveRecord models in migrations unless you re-define them within the migration
17. …otherwise the migration fails when you later remove/rename the AR class
18. BETTER SOLUTION: use bootstrapping until deployed!!!
19. AJAX only for sub-components of an object, and avoid over-use

## CONTROLLER CODING STANDARDS

1.  Before filter should be added at the top of controller.
2.  Before filter implementation should be immediate after filter declaration
3.  Standard rails actions
4.  Own custom actions
5.  Inter-related actions should be clubbed together.
6.  please, try to use of protected, private methods and they should be declared at the bottom of page in order.
7.  Controller Actions should be like :
    (use 2 spaces for indent and not tabs)

```ruby
def self.published_jokes
  find(:all, :conditions => "published = 1")
end
```

## MODEL CODING STANDARDS

```ruby
#======= HEADER SECTION=========

# SCHEMA INFORMATION WILL BE HERE
# REQUIRE FILES WILL GO HERE

class Model < ActiveRecord::Base

 #======== TOP ===================

 # LIBRARY OR INCLUDE METHODS
 # MODEL RELATIONSHIPS
 # VIRTUAL ATTRIBUTES
 # ACTIVE RECORD VALIDATIONS

 #======== MIDDLE ===============

 # CUSTOM VALIDATIONS
 # PUBLIC METHODS

 #======== BASE ==================

 # PROTECTED METHODS
 # PRIVATE METHODS

end
```
