---
layout: post
title: "Paypal payments integration with rails"
date: 2011-12-30T18:00:00+05:30
comments: false
categories:
 - payment gateway
 - paypal
 - rails
---
Paypal standard website payment service allows online payment transactions for websites. 
Before implementing payments inside rails app needs to have following things in place - 

1. [Register Paypal sandbox account](http://developer.paypal.com/)
2. Paypal Merchant account api credentials i.e. login, password, signature, application_id
3. Paypal Buyer account creds to test payments

Bundle Install
```
# Gemfile     
gem 'activemerchant 
```
Gateway config
```
# config/gateway.yml 
development: &development     
  mode: test     
  login: rana_1317365002_biz_api1.gmail.com     
  password: '1311235050'     
  signature: ACxcVrB3mFChvPIe8aDWQlLhAPN46oPBQCj7rJWPza6CDZmBURg.     
  application_id: APP-76y884485P519543T  

production:    
  <<: *development

test:
  <<: *development
```
New Payment Form
```haml
= form_for @payment ||= Payment.new, :url => pay_bill_url, :html => {:id => :payForm} do |p|    
  = p.text_field :amount   
  = p.submit 'Pay' 
```

Generate & Migrate Payment Model
```
rails g model payment status:string amount:float transaction_number:string   
rake db:migrate 
```

Payment Model
```ruby
# app/models/payment.rb  
class Payment < ActiveRecord::Base

  PROCESSING, FAILED, SUCCESS = 1, 2, 3

  validates :amount, :presence => true, :numericality => { :greater_than => 0 }    
  def self.conf
    @@gateway_conf ||= YAML.load_file(Rails.root.join('config/gateway.yml').to_s)[Rails.env]   
  end    
  
  ## Paypal    
  def setup_purchase(options)     
    gateway.setup_purchase(amount * 100, options)   
  end    
  
  def redirect_url_for(token)      
    gateway.redirect_url_for(token)   
  end 
  
  def purchase(options={}) 
    self.status = PROCESSING  
    #:ip       => request.remote_ip,
    #:payer_id => params[:payer_id],
    #:token    => params[:token]
    response = gateway.purchase(amt, options)      
    if response.success?       
      self.transaction_num = response.params['transaction_id']       
      self.status = SUCCESS     
    else       
      self.status = FAILED     
    end     
    return self   
  rescue Exception => e     
    self.status = FAILED     
    return self   
  end    

  private   
  def gateway 
    ActiveMerchant::Billing::Base.mode = auth['mode'].to_sym 
    ActiveMerchant::Billing::PaypalExpressGateway.new(
      :login => auth['login'], :password => auth['password'],
      :signature => auth['signature']) 
  end

  def auth 
    self.class.conf 
  end
end 
``` 
Billing routes
```ruby
## Callback URL   
match '/billing/paypal/:id/confirm', :to => 'billing#paypal', :as => :confirm_paypal   
## Create payment   
match '/billing', :to => 'billing#create', :as => :pay_bill   
## Request URL   
match '/billing/paypal/:id', :to => 'billing#checkout', :as => :billing   
match '/billing/thank_you/:id', :to => 'billing#checkout', :as => :billing_thank_you 
```

Billing Controller
```ruby
# app/controllers/billing_controller.rb
class BillingController < ApplicationController
  before_filter :get_payment, :only => [:checkout, :paypal, :thank_you]      
  
  def create     
    @payment = Payment.new params[:payment]     
    if @payment.save       
      ## Paypal Checkout page       
      redirect_to billing_url    
    else     
      render :action => :new    
    end 
  end    
  
  # ASSUMPTION   # payment is valid i.e. amount is entered   
  def checkout    
    response = @payment.setup_purchase(:return_url => confirm_paypal_url(@payment), :cancel_return_url => root_url)     
    redirect_to @payment.redirect_url_for(response.token)   
  end    
  
  ## CALL BACK   
  def paypal    
    @payment = @payment.purchase(:token => params[:token], :payer_id => params[:PayerID], :ip => request.remote_ip)    
    @payment.save    
    redirect_to thank_you_billing_url(@order)  
  end    
  
  private   
  def get_payment     
    @payment = Payment.find_by_id(params[:id])     
    @payment && @payment.valid? || invalid_url   
  end 
end
```

Views
```haml
# app/views/billing/thank_you.html.haml  
- if @payment.success?   
  %p The transaction is successfully completed 
- else   
  %p The transaction failed 
```   
