---
layout: post
title: "csv file import / export in rails 3"
date: 2012-01-27T14:28:00+05:30
comments: false
categories:
 - rails 3
 - faster-csv
---
CSV (comma separated values) files are frequently used to import/export data. 

In rails 3, `FasterCSV` comes as default and below is the way to upload csv files inside rails applications. The code below will also show you how to generate csv in memory, parse on csv data, skip header, iterate over records, save records inside db, export upload error file and many more. 

#### Upload form

```haml
= form_tag upload_url, :multipart => true do
  %label{:for => "file"} File to Upload
  = file_field_tag "file"
  = submit_tag
```
<!--more-->
Assume `upload_url` maps to import action of customers controller 

#### Controller code 

```ruby
class CustomersController < ApplicationController  
  [...] 
  def import
    if request.post? && params[:file].present?
      infile = params[:file].read
      n, errs = 0, []

      CSV.parse(infile) do |row|
        n += 1
        # SKIP: header i.e. first row OR blank row
        next if n == 1 or row.join.blank?
        # build_from_csv method will map customer attributes & 
        # build new customer record
        customer = Customer.build_from_csv(row)
        # Save upon valid 
        # otherwise collect error records to export
        if customer.valid?
          customer.save
        else
          errs << row
        end
      end
      # Export Error file for later upload upon correction
      if errs.any?
        errFile ="errors_#{Date.today.strftime('%d%b%y')}.csv"
        errs.insert(0, Customer.csv_header)
        errCSV = CSV.generate do |csv|
          errs.each {|row| csv << row}
        end
        send_data errCSV,
          :type => 'text/csv; charset=iso-8859-1; header=present',
          :disposition => "attachment; filename=#{errFile}.csv"
      else
        flash[:notice] = I18n.t('customer.import.success')
        redirect_to import_url #GET
      end
    end
  end
  [...]
end
```
#### Customer model 

```ruby
class Customer < ActiveRecord::Base
  scope :active, where(:active => true)
  scope :latest, order('created_at desc')
  
  def self.csv_header
    "First Name,Last Name,Email,Phone,Mobile, Address, FAX, City".split(',')
  end
  
  def self.build_from_csv(row)
    # find existing customer from email or create new
    cust = find_or_initialize_by_email(row[2])
    cust.attributes ={:first_name => row[0],
      :last_name => row[1],
      :email => row[3],
      :phone => row[4],
      :mobile => row[5],
      :address => row[6],
      :fax => row[7],
      :city => row[8]}
    return cust
  end
  
  def to_csv
    [first_name, last_name, email, phone, mobile, address, fax, city]
  end
end
```

## Export customer records in CSV format

Below code loads customer records from database then generate `csv_data` inside memory and exports data to browser using `send_data` method.

Note: As we are not writing on file system hence code can easily work heroku.
```ruby
def export
  # CRITERIA : to select customer records
  #=> Customer.active.latest.limit(100)
  custs = Customer.limit(10)
  filename ="customers_#{Date.today.strftime('%d%b%y')}"
  csv_data = FasterCSV.generate do |csv|
    csv << Customer.csv_header
    custs.each do |c| 
      csv << c.to_csv
    end 
  end 
  send_data csv_data,
    :type => 'text/csv; charset=iso-8859-1; header=present',
    :disposition => "attachment; filename=#{filename}.csv"
end 
```
