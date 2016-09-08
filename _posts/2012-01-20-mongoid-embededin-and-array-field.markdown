---
layout: post
title: "Mongoid embeded_in and Array field management"
date: 2012-01-20T01:12:00+05:30
comments: false
categories:
 - mongoid
 - rails
---
Previous post explains on [mongoid document array field and rails form implementation](http://www.funonrails.com/2012/01/mongoid-array-field-and-rails-form)

Below example shows rails form integration of array field of embedded mongoid document

Lets consider scenario that `student` embeds one `family` who has many `assets`
```ruby
class Student
  include Mongoid::Document

  field :name
  field :phone
  
  embeds_one  :family

  validates_associated :family
  accepts_nested_attributes_for :family
end
```
<!--more-->
```ruby
class Family
  include Mongoid::Document
  ASSETS = ['flat', 'car', 'business', 'bunglow', 'cash']
  
  field :members, type: Integer
  field :assets, type: Array
  field :religon

  embedded_in :student
end
```
Brief controller code 
```ruby
class StudentsController < ApplicationController
 def new
   @student = Student.new
   @student.family ||= @student.build_family
 end

 def create
   @student = Student.new(params[:student])
   @student.family.assets.reject!(&:blank?)
   if @student.save
     [...]
   else
     render :action => :new
   end
 end
end
```
view form will look like- 
```haml
= form_for(@student) do |s|
  = s.text_field :name
  = s.text_field :phone
  - s.fields_for :family do |f|
    = f.text_field :members
    = f.text_field :religion
    - Family::ASSETS.each do |asset|
      /Here f.object_name #=> student[family]
      = f.check_box :assets, :name => "#{f.object_name}[assets][]", asset
```
