---
layout: post
title: "Authorize Net (SIM) payment integration with rails"
date: 2011-12-30T16:16:00+05:30
comments: false
categories:
 - payment gateway
 - rails
 - authorize-net
---
Authorize Net SIM gateway transaction skips merchant side creditcard details form and directs transaction to be take place on gateway server.
```
# Gemfile
gem 'authorize-net'
```
Register for authorize net sandbox account click [here](https://developer.authorize.net/testaccount/)

Payment gateway credentials
```
# config/gateway.yml
development: &development
    mode: test
    login: 9gdLh6T
    key: 67fu45xw6VP92LX1

production:
   <<: *development

test:
   <<: *development
```

Generate & Migrate Payment Model
```ruby
rails g model payment status:string amount:float transaction_number:string
rake db:migrate
```

SIM gateway methods extracted and added to payment model 
```ruby
# app/models/payment.rb
class Payment < ActiveRecord::Base
  
  PROCESSING, FAILED, SUCCESS = 1, 2, 3
  
  validates :amount, :presence => true, :numericality => { :greater_than => 0 }

  def self.conf
    @@gateway_conf ||= YAML.load_file(Rails.root.join('config/gateway.yml').to_s)[Rails.env]
  end
  
  def success?
    self.status == SUCCESS
  end

  ## Authorize :: SIM
  def setup_transaction(options ={})
    options.merge!(:link_method => AuthorizeNet::SIM::HostedReceiptPage::LinkMethod::POST)
    t = AuthorizeNet::SIM::Transaction.new(
      auth['login'], auth['key'], amount,
      :hosted_payment_form => true,
      :test => auth['mode']
    )
    t.set_hosted_payment_receipt(AuthorizeNet::SIM::HostedReceiptPage.new(options))
    return t
  end

  def auth
    self.class.conf
  end
end
```

Payment routes 
```ruby
## Callback URL
match '/billing/:id/confirm', :to => 'billing#authorize', :as => :confirm_billing
 
## Request URL
match '/billing/:id', :to => 'billing#checkout', :as => :billing
match '/billing/:id/thank_you', :to => 'billing#thank_you', :as => :thank_you_billing
```

Billing controller 
```ruby
# app/controllers/billing_controller.rb 
class BillingController < ApplicationController
  helper :authorize_net

  before_filter :get_order, :only => [:checkout, :authorize, :thank_you]

  def checkout
    # ASSUMPTION order is valid means amount is entered
    @transaction = @order.setup_transaction(
      {:link_text => 'Continue',
        :link_url => confirm_billing_url(@order)})
  end

  ## CALL BACK
  def authorize
    resp = AuthorizeNet::SIM::Response.new(params)
    if resp.approved?
      @order.status = Payment::SUCCESS
      @order.transaction_num = resp.transaction_id
    else
      @order.status = Payment::FAILED 
    end
    @order.save(:validate => false)
    redirect_to thank_you_billing_url(@order)
  end

  private
  def auth
    Payment.conf
  end

  def get_order
    @order = Payment.find_by_id(params[:id])
    @order && @order.valid? || invalid_url
  end
end
```

View Forms
```haml
# app/views/billing/checkout.html.haml 
= form_for :sim_transaction, :url => AuthorizeNet::SIM::Transaction::Gateway::TEST, :html => {:id => :authForm} do |f|
  = sim_fields(@transaction)
:javascript
  $(document).ready(function(){
    $('#authForm').submit();
  })
```
```haml
# app/views/billing/thank_you.html.haml 
- if @order.success?
  %p The transaction is successfully completed
- else
  %p The transaction failed
```
