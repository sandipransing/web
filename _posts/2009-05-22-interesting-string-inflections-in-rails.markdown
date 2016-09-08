---
layout: post
title: "Intresting String inflections in rails"
date: 2009-05-22T19:58:00+05:30
comments: false
categories:
 - ruby
 - rails
---

Here is list of string inflections :

* camelcase
* camelize
* classify
* constantize
* dasherize
* demodulize
* foreign_key
* humanize
* parameterize
* pluralize
* singularize
* tableize
* titlecase
* titleize
* underscore

#### camelcase()
```
camelcase(first_letter = :upper)
```

Alias for camelize
```ruby
camelize(first_letter = :upper)
```
By default, camelize converts strings to UpperCamelCase. If the argument to camelize is set to :lower then camelize produces lowerCamelCase.

camelize will also convert ’/’ to ’::’ which is useful for converting paths to namespaces.
```
"active_record".camelize # => "ActiveRecord"
"active_record".camelize(:lower) # => "activeRecord"
"active_record/errors".camelize # => "ActiveRecord::Errors"
"active_record/errors".camelize(:lower) # => "activeRecord::Errors"
```
This method is also aliased as camelcase

#### classify()

Create a class name from a plural table name like Rails does for table names to models. Note that this returns a string and not a class. (To convert to an actual class follow classify with constantize.)
```ruby
"egg_and_hams".classify # => "EggAndHam"
"posts".classify # => "Post"

# Singular names are not handled correctly.

"business".classify # => "Busines"
```

#### constantize()

constantize tries to find a declared constant with the name specified in the string. It raises a NameError when the name is not in CamelCase or is not initialized.

**Examples :**

"Module".constantize # => Module
"Class".constantize # => Class

#### dasherize()

Replaces underscores with dashes in the string.
```
"puni_puni" # => "puni-puni"
```

#### demodulize()

Removes the module part from the constant expression in the string.

```ruby
"ActiveRecord::CoreExtensions::String::Inflections".demodulize # => "Inflections"
"Inflections".demodulize # => "Inflections"
```

#### foreign_key(separate_class_name_and_id_with_underscore = true)

Creates a foreign key name from a class name. separate_class_name_and_id_with_underscore sets whether the method should put "_" between the name and 'id'.

**Examples :**

```ruby
"Message".foreign_key # => "message_id"
"Message".foreign_key(false) # => "messageid"
"Admin::Post".foreign_key # => "post_id"
```

#### humanize()

Capitalizes the first word, turns underscores into spaces, and strips `_id`. Like titleize, this is meant for creating pretty output.
```ruby
"employee_salary" # => "Employee salary"
"author_id" # => "Author"
````
#### parameterize()

Replaces special characters in a string so that it may be used as part of a ‘pretty’ URL.
**Examples :**

```ruby
class Person
  def to_param
  "#{id}-#{name.parameterize}"
  end
end

@person = Person.find(1)
# => Donald E. Knuth
```
#### pluralize()

Returns the plural form of the word in the string.
```ruby
"post".pluralize # => "posts"
"octopus".pluralize # => "octopi"
"sheep".pluralize # => "sheep"
"words".pluralize # => "words"
"the blue mailman".pluralize # => "the blue mailmen"
"CamelOctopus".pluralize # => "CamelOctopi"
```

#### singularize()

The reverse of pluralize, returns the singular form of a word in a string.

```ruby
"posts".singularize # => "post"
"octopi".singularize # => "octopus"
"sheep".singularize # => "sheep"
"word".singularize # => "word"
"the blue mailmen".singularize # => "the blue mailman"
"CamelOctopi".singularize # => "CamelOctopus"
```

#### tableize()

Creates the name of a table like Rails does for models to table names. This method uses the pluralize method on the last word in the string.
```ruby
"RawScaledScorer".tableize # => "raw_scaled_scorers"
"egg_and_ham".tableize # => "egg_and_hams"
"fancyCategory".tableize # => "fancy_categories"
```
#### titlecase()

Alias for titleize
titleize()

Capitalizes all the words and replaces some characters in the string to create a nicer looking title. titleize is meant for creating pretty output. It is not used in the Rails internals.

titleize is also aliased as titlecase.
```ruby
"man from the boondocks".titleize
# => "Man From The Boondocks"
"x-men: the last stand".titleize
# => "X Men: The Last Stand"
```
This method is also aliased as titlecase

#### underscore()

The reverse of camelize. Makes an underscored, lowercase form from the expression in the string.
underscore will also change ’::’ to ’/’ to convert namespaces to paths.

```ruby
"ActiveRecord".underscore
# => "active_record"
"ActiveRecord::Errors".underscore
# => active_record/errors
```

#### Remove special characters from string 
```ruby
"STRING"..gsub(/[^a-zA-Z 0-9]/, "")

# escape special chars before save
def before_save
  self.name.gsub(/[^a-zA-Z 0-9]/, "")
end
```
