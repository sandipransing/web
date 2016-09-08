---
layout: post
title: "Customizing rails default form builder"
date: 2011-12-29T01:29:00+05:30
comments: false
categories:
 - rails
 - form-builder
---

Customizing default rails form builder to adopt for labels, input fields, errors, hints, etc. in order to build forms just in minutes 

```ruby
# app/helpers/app_form_builder.rb
class AppFormBuilder < ActionView::Helpers::FormBuilder  

  HELPERS = %w[check_box text_field text_area password_field select date_select datetime_select file_field collection_select state_select label calendar_date_select]
  def self.create_tagged_field(method_name)
    define_method(method_name) do |name, *args|
      errs = object.errors.on(name.to_sym) if object && object.errors

      # initialize some local variables
      if args.last.is_a?(Hash)
        label = args.last.delete(:label)
        suffix = args.last.delete(:suffix)
        klass = args.last.delete(:class)
        req = args.last.delete(:required)
      end
      label = 'none' if method_name == 'hidden_field' 
      label ||= name.to_s.titleize
      label = nil if label == 'none'

      klass = klass ? [klass] : []
      # Custom class if it exists
      if method_name =~ /text_field|check_box|select/
        klass << method_name
      end
      klass << 'f' #A default selector
      klass << 'error' if errs.present?
      klass = klass.join(' ')

      # Required Field Notations
      if req == 'all' || (req == 'new' && object.new_record?)
        label << @template.content_tag(:span, :*, :class => :req)
      end

      suffix = @template.content_tag(:label, suffix) if suffix.present?
      label = @template.content_tag(:label, label) if label.present?
      errs = @template.content_tag(:span, errs.to_s, :class => :message) if errs.present?
      reverse = true if method_name == 'check_box'
      if reverse
        content = "#{super} #{suffix} #{label} #{errs}"
      else
        content = "#{label} #{super} #{suffix} #{errs}"
      end

      @template.content_tag(:div, content, :class => klass)
    end
  end

  HELPERS.each do |name|
    create_tagged_field(name)
  end
end
```
