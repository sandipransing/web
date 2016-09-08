---
layout: post
title: "twitter-bootstrap form builder for rails"
date: 2012-01-17T05:39:00+05:30
comments: false
categories:
 - bootstrap
 - form-builder
 - rails 3
---
[twitter-bootstrap](twitter.github.com/bootstrap/) is pluggable css suit provided by twitter. 
To know more about how to get started on it click [here](http://www.funonrails.com/2011/11/rails-311-haml-sass-jquery-coffee-rails).

Below post will help you out in getting started bootstrap css with rails app. One need to add below files to helpers directory.
MainForm can be used as base version of form builder and can be overriden for its subsequent use inside other custom form builders. 

1. Main Form
```ruby
# app/helpers/main_form.rb 
class MainForm < ActionView::Helpers::FormBuilder # NestedForm::Builder
  CSS = {
    :label => 'label-control',
    :hint => 'hint',
    :hint_ptr => 'hint-pointer',
    :error => 'help-inline',
    :field_error => 'error',
    :main_class => 'clearfix'
  }

  FIELDS = %w(radio_button check_box text_field text_area password_field select file_field collection_select email_field date_select)
  
  def main_class(error=nil)
    return CSS[:main_class] unless error
    [CSS[:main_class], CSS[:field_error]].join(' ')
  end

  def required(name)
    object.class.validators_on(name).map(&:class).include?(ActiveModel::Validations::PresenceValidator) rescue nil
  end

  def cancel(options={})
    link = options.fetch(:return, "/")
    @template.content_tag(:a, "Cancel", :href => link, :class => "btn_form button np_cancel_btn #{options[:class]}")
  end

  def submit(value="Save", options={})
    options[:class] = "send_form_btn #{options[:class]}"
    super
  end

  def label_class
    {:class => CSS[:label]}
  end

  def label_tag(attribute, arg)
    # Incase its a mandatory field, the '*' is added to the field.
    txt = arg[:label] && arg[:label].to_s || attribute.to_s.titleize
    txt<< '*' if(arg[:required] || required(attribute)) && arg[:required] != false
    label(attribute, txt, label_class)
  end

  def error_tag(method_name, attribute)
    errs = field_error(method_name, attribute)
    @template.content_tag(:span, errs.first, :class => CSS[:error]) if errs.present?
  end

  def field_error(method_name, attribute)
    return if @object && @object.errors.blank?
    return @object.errors[attribute] if method_name != 'file_field'
    @object.errors["#{attribute.to_s}_file_name"] | @object.errors["#{attribute.to_s}_file_size"] | @object.errors["#{attribute.to_s}_content_type"]
  end

  def hint_tag(txt)
    hintPtr = @template.content_tag(:span, '', :class => CSS[:hint_ptr])
    hintT = @template.content_tag(:span, txt + hintPtr, {:class => CSS[:hint]}, false) 
  end
 
  def spinner_tag
    @template.image_tag('spinner.gif', :class => :spinner,:id => :spinner) 
  end
end  
```
<!--more-->

ZeroForm is custom form builder which is inherited from `main_form` and its going to be actually used inside forms. 
Feel free to make custom form related changes inside this

```ruby
# app/helpers/zero_form.rb
class ZeroForm < MainForm
  # Overridden label_class here as we dont need class to be applied
  def label_class
    {}
  end

  def self.create_tagged_field(method_name)
    define_method(method_name) do |attribute, *args|
      arg = args.last && args.last.is_a?(Hash) && args.last || {}

      # Bypass form-builder and do your own custom stuff!
      return super(attribute, *args) if arg[:skip] && args.last.delete(:skip)

      errT = error_tag(method_name, attribute)
      labelT = label_tag(attribute, arg)

      mainT = super(attribute, *args)
      baseT = @template.content_tag(:div, mainT + errT)

      hintT = hint_tag(arg[:hint]) if arg[:hint]
      spinnerT = spinner_tag if arg[:spinner]

      allT = labelT + baseT + spinnerT + hintT
      @template.content_tag(:div, allT, :class => main_class(errT))
    end
  end

  FIELDS.each do |name|
    create_tagged_field(name)
  end
end
```
In order to use Nested Forms you need to extend MainForm with NestedForm Builder

**Integrate NestedForm with FormBuilder**

```ruby
class MainForm < NestedForm::Builder
end
```
**View Form**
```haml
= form_for @address ||= Address.new, :builder => ZeroForm do |f|
  = f.text_field :street_address
  = f.text_area :detail_address, :rows => 2
  = f.text_field :city
  = f.select :state, %w(US IN AUS UK UKRAINE)
  = f.submit 'Save & Continue', :class => 'btn primary'
  = link_to 'Skip »', '#'
```
