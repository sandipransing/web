---
layout: post
title: "Authorize Net (AIM) payment integration with rails"
date: 2011-12-28T17:15:00+05:30
comments: false
categories:
 - payment gateway
 - activemerchant
 - rails
 - authorize-net
---
Authorize Net (AIM) method enables internet merchants to accept online payments via credit card.
Below post will show you how to integrate authorize net payment gateway inside rails app to accept online payments using activemerchant library.

```
# Gemfile
gem 'activemerchant', :require => 'active_merchant'
```

Register for authorize net sandbox account click [here](https://developer.authorize.net/testaccount/)

Payment gateway credentials
```
# config/authorize_net.yml
development: &development
    mode: test
    login: 9gdLh6T
    key: 67fu45xw6VP92LX1

production:
   <<: *development

test:
   <<: *development
```

Payment & creditcard form 
```haml
# app/views/payments/new
= form_for @payment, :url => payments_url do |f|
  = f.text_field :amount
  = fields_for :creditcard, @creditcard do |cc|
    = cc.text_field :name
    = cc.text_field :number
    = cc.select :month, Date::ABBR_MONTHNAMES.compact.each_with_index.collect{|m, i| [m, i+1]}, {:prompt => 'Select'}
    = cc.select :year, Array.new(15){|i| Date.current.year+i}, {:prompt => 'Select'}
    = cc.text_field :verification_value
  = f.submit 'Pay'
```

Payments Controller 
```ruby
# app/controllers/payments_controller.rb 
class PaymentsController < ApplicationController

  def new
    @payment = Payment.new
    @creditcard = ActiveMerchant::Billing::CreditCard.new
  end

  def create
    @payment = Payment.new(params[:payment])
    @creditcard = ActiveMerchant::Billing::CreditCard.new(params[:creditcard])
    @payment.valid_card = @creditcard.valid?
    if @payment.valid? 
      @payment = @payment.process_payment(@creditcard)
      if @payment.success?
        @payment.save
        flash[:notice] = I18n.t('payment.success')
        redirect_to payments_url and return
      else
        flash[:error] = I18n.t('payment.failed')
      end
    end
    render :action => :new
  end
end
```

Generate & Migrate Payment Model
```ruby
rails g model payment status:string amount:float transaction_number:string
rake db:migrate
```

Payment Model
```ruby
# app/models/payment.rb 
class Payment < ActiveRecord::Base

  PROCESSING, FAILED, SUCCESS = 1, 2, 3

  validates :valid_card, :inclusion => {:in => [true], :message => 'Invalid Credit Card'}
  validates :amount, :presence => true, :numericality => { :greater_than => 0 }

  def process_payment(creditcard)
    ActiveMerchant::Billing::Base.mode = auth['mode'].to_sym
    self.status = PROCESSING
    response = gateway.purchase(amount * 100, creditcard)

    if response.success?
      self.transaction_number = response.subscription_id
      self.status = SUCCESS
    else
      self.status = FAILED
    end
    return self
  rescue Exception => e
    self.status = FAILED
    return self
  end

  def success?
    self.status == SUCCESS
  end

  private
  def gateway
    ActiveMerchant::Billing::AuthorizeNetGateway.new(
      :login    => auth['login'],
      :password => auth['key'])
  end

  def auth
    @@auth ||= YAML.load_file("#{Rails.root}/config/authorize_net.yml")[Rails.env]
  end
end
```
