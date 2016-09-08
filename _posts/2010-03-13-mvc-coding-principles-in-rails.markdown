---
layout: post
title: "MVC coding principles in rails"
date: 2010-03-13T05:26:00+05:30
comments: false
categories:
 - best-practices
 - rails
---

I believe this post should clear basic coding standards before getting started with rails development.

I just came with conversion started for declaring variables on rubyonrailstalk google group and that ended up with&nbsp;exploring the concepts of rails MVC coding principles.
so, i thought it would be nice if they gets summarized somewhere.

The question raised was:

{% blockquote @capsized %}
How to declare variables like arrays so that they are available everytime that layout or its related partials are used. Putting `@myarray = MyArray.all` into every action in every controller doesn't seem very dry, so I guess I'm just looking for a very simple straighforward convention, but can't seem to find it documented anywhere or figure it out
{% endblockquote %}

Before getting into actual conversion, I would like to notedown what rails is.

> Rails is web framework developed in ruby language. The main purpose behind is to make programmers life easy and development would be a fun. It is  built over MVC framework and philosophy includes DRY, Convention over configuration and REST. 

Adding conversation as it is here:

{% blockquote @Xavier  %}
Noria Normally something like that goes into a high-level filter, like one  in ApplicationController.
{% endblockquote %}
 
Put a before_filter in your ApplicationController. 
 
```ruby @Andy
 class ApplicationController 
  before_filter :set_my_array
  private
  def set_my_array
    @myarray = MyArray.all
  end
end
```

{% blockquote @Colin %}
If it is literally something as simple as MyArray.all I believe there
is nothing wrong with calling the model direct from the view.
{% endblockquote %}

And here started the debate conversation. First of all, i was also convinced with this suggestion but next reply started conversion.

{% blockquote @Andy %}
It's dirty, horrible, bad form, breaks the separation of layers...
Don't call the model from the view!
{% endblockquote %}

{% blockquote @Sharagoz %}
Beware of the MVC police Colin, this suggestion will certanly not get
good housekeeping seal of approval :D
I agree through. I'm not gonna add a before filter just to set
MyArray.all into a class variable. I'd rather call it directly and claim
to be pragmatic.
{% endblockquote %}

{% blockquote @Marnen %}
But you are wrong.  The view should never, ever, ever touch the database.
Claim all you like.  The fact is that in MVC architecture, database
queries don't belong in the view. A before_filter is the proper place for this.
{% endblockquote %}

{% blockquote @Colin %}
Is it considered ok to call model methods if they do not touch the db,
or are model methods forbidden also?
{% endblockquote %}

{% blockquote @Andy %}
I would say the view can call instance methods of the model (attributes - real and virtual) but no class methods. So: is OK, but:
is not.  It's a bit of a contrived example, but there you go.  Accessing the model through instance variables you've created is OK, going directly to the model bypassing the controller is not.
{% endblockquote %}

I think it is appropriate for the view to call methods on the objects passed in by the controller, provided that these methods do not change the model or touch the database.
Example:
```ruby Good: @Marnen
# controller
def my_action
 @person = Person.find(params[:id])
end
#my_action.html.erb
```

Here started conclusion on topic:
{% blockquote @Robert %}
Good example. I just want to throw in a couple more twists and see what
you think.
1.  What about methods on models that change themselves in some way?
Suppose the last_viewed_at method returned a previously stored time,
then updated the model to store a new current time. Maybe a bad example,
but I hope you get my meaning.
2.  What about aggregating class methods like count, sum or avg?
Obviously a class methods and does touch the database. I assume it would
be better to let the controller deal with stuff like this.
```ruby
# Controller
 @person_count = Person.count
```
{% endblockquote %}

{% blockquote @Colin %}
I don't know what you mean by dirty, it saves several lines of code
and when looking at the view code it is easier to see what is
happening than to see a variable that has to be hunted for in a filter
somewhere to find out what it is.
It does not break the separation of layers any more than calling an
instance method of a model does when using something like &gt; Don't call the model from the view!
`@person.name` is a model call from the view
{% endblockquote %}

{% blockquote @Josh %}
But think how many lines of code you are going to have to go edit when you realize that you need to change it. 
Also, I wouldn't consider Person.all to be more clean than @people. What if you need to exclude some? Person.all :conditions =&gt; {whatever}, if you are just using a before filter, it is easy to override, you can override it for any given controller, and for any given controller method. If it's hard coded into the view, then that view has to serve everybody's wishes, it ends up having to know how it is to be used, and having lots of brittle conditional code for each of these situations.
This is why the controller must be responsible for supplying the appropriate data to the view, not the view being responsible for creating it's own data. 
It might start as innocently as `Person.all`, it can easily turn into
```ruby
if this
  Person.all
elsif that
  OtherPerson.all
else
  Person.all + OtherPerson.all
end
```
{% endblockquote %}

This took another turn in conversion -

{% blockquote @Andy %}
Did you misunderstand my post?  I was arguing for putting it in a filter rather than in the view (hence saying the 4 lines of before_filter, private, def and end was worth it).
It sounds like - when you start with "But" - that you disagree, but the rest of your post seems to be arguing from the same side as my posts.
{% endblockquote %}

And finally Colin ended up conversion with conclusion -

{% blockquote %}
An excellent post if I may say so that brings out the salient points I think.  Can the issues be summarised as follows?
The controller should provide all the data that the view should display in instance variables (@person for example).
The view is expected to understand the structure of the objects and so can access attributes (virtual or otherwise) of the objects.
If the model needs to access the db in order to provide an attribute value, or accessing the attribute has some side effect that affects the db, then this is ok, providing the view does not 'know' that the side effect or db access is happening. (Not very well written but I hope you know what I mean).
The view must not call any method of the model who's purpose is to perform an action rather than return a value.
The view should not make any explicit use of model classes.  For example there should be no reference to Person or any other model class 
{% endblockquote %}
