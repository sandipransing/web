---
layout: post
title: "Zipcode validation using geokit in rails"
date: 2010-08-21T04:58:00+05:30
comments: false
categories:
 - api
 - rails
 - geokit
---

1.  **Install geokit gem**
```
gem install geokit
```
OR  
```ruby
# Add following line inside rails initialize block
Rails::Initializer.run do |config|
 config.gem 'geokit'
end
```
And then run command
```
rake gems:install
```
2.  **Consider User model with zipcode as attribute field**
```ruby
include Geokit::Geocoders
class User < ActiveRecord::Base
  set_table_name :users
  validate_presence_of :zipcode
  validate :request_zipcode_validation_using_geokit, :if => :zipcode

  private
  def request_zipcode_validation_using_geokit
    # Method request google api for location
    # if location found then zipcode is valid otherwise
    # add validation error on zipcode field
    # as it method contacts with google api and takes time
    # to return result, poll request only when zipcode gets
    # changed
    poll = true # default true for new objects
    if self.id ## this means already existing user and zipcode is valid last time
      # Hack to find where zipcode got modified or not 
      # old_user = User.find self.id
      poll = false if old_user.zipcode == self.zipcode
    end

    # Actual requesting api to return location associated with zipcode
    if poll
      loc = MultiGeocoder.geocode(self.zip_code)
    end
    # Add Validation Error if location is not found
    errors.add(:zip_code, "Unable to geocode your location from zipcode entered.") unless loc.success
  end
end
```
**Please note that same method can also be used to validate state, city and country.
Again we can use combination of fields to validate each other.
Like -**

1.  **Based on country entered, state validation**
2.  **Based on state, city validation**
3.  **Based on city, zipcode validation**
or
4.  **Based on zipcode and country, state and city validation**

Here is another method to validate state and city based on zipcode and country.
Lets take example of 'US'
```ruby
def request_state_and_city_validation_based_on_zipcode
  poll = true # default true for new objects
  if self.id ## this means already existing user and all attributes were valid last time
    # Hack to find any one of location attribute got modified
    # old_user = User.find self.id
    loc_attrs = %w{zipcode state city} # keep in mind country US is default assumed
    if loc_attrs.all? {|attr| self.attribute_for_inspect(attr) == old_user.attribute_for_inspect(attr)}
      self.poll = false
    end
  end

  # Actual requesting api to return location associated with zipcode
  if poll
    loc = MultiGeocoder.geocode("#{self.zip_code}, US")
  end
  # Add Validation Error if location is not found
  unless loc.success
    errors.add(:zip_code, "Unable to geocode your location from zipcode entered.")
  else
    # Validate state and city fields in compare to loc object returned by geocode
    errors.add(:state, "State doesn't matches with zipcode entered") if self.state != loc.state
    errors.add(:city, "City doesn't matches with zipcode entered") if self.city != loc.city
  end
end
```
**Note** \*\*
If you are subscriber of blog and not displaying post correctly. I request you to visit post on blog itself. Somehow style is not getting correctly in email. I will try to fix this problem asap.

**Upcoming Posts**

1.  Geokit finders: Find locations in/within/beyond particular radius from specified location using acts_as_mappable plugin
2.  Customizing authlogic for multiple sessions i.e. using different models for role based authentication.
