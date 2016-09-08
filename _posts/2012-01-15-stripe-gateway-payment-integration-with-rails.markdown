---
layout: post
title: "stripe payment gateway integration with rails"
date: 2012-01-15T00:53:00+05:30
comments: true
categories:
 - payment gateway
 - stripe
 - rails 3
---
Stripe is simple website payment solution and its very easy to easy setup.
It currently supports only in US and seems to be very popular compared to other payment gateways because of its api & pricing

Stripe API provides -

1. charge (regular payments)
2. subscription (recurring payments)
3. managing customers (via stripe_customer_token)

**What you need to do ?**

Create a stripe account by providing email address and password. There after go to the [manage account page](https://manage.stripe.com/account) to obtain stripe public & api keys.
Rails Integration

Rails Integration
```
# Gemfile
gem stripe
```

Add api key and public key
```ruby
# config/initializers/stripe.rb

Stripe.api_key = "rGaNWsIG3Gy6zvXB8wv4rEcizJp6XjF5"
STRIPE_PUBLIC_KEY = "vk_BcSyS2qPWdT5SdrwkQg0vTSyhZgqN"
```
View
```haml
# app/views/layouts/application.html.haml

= javascript_include_tag 'https://js.stripe.com/v1/'
= tag :meta, :name => 'stripe-key', :content => STRIPE_PUBLIC_KEY
```
Payment Form
```haml
# app/views/payments/new.html.haml 

#stripe_error
  %noscript JavaScript is not enabled and is required for this form. First enable it in your web browser settings.

= form_for @payment ||= Payment.new, :html => {:id => :payForm} do |p|
  = p.hidden_field :stripe_card_token
  .field
    = p.text_field :amount
  .credit_card_form
    %h3.title
      Enter Credit Card
    - if @payment.stripe_card_token.present?
      Credit card has been provided.
    - else
      .field
        = label_tag :card_number, "Credit Card Number"
        = text_field_tag :card_number, nil, name: nil
      .field
        = label_tag :card_code, "Security Code (CVV)"
        = text_field_tag :card_code, nil, name: nil
      .field
        = label_tag :card_month, "Expiry Date"
        = select_month nil, {add_month_numbers: true}, {name: nil, id: "card_month"}
        = select_year nil, {start_year: Date.today.year, end_year: Date.today.year+15}, {name: nil, id: "card_year"}
```

Javascript Code
```javascript
# app/views/payments/new.js 

var payment;
jQuery(function() {

  Stripe.setPublishableKey($('meta[name="stripe-key"]').attr('content'));
  return payment.setupForm();
});

payment = {

  setupForm: function() {

    $('.head').click(function() {
      $(this).css('disabled', true);

      if($('#payment_stripe_card_token').val()){
        $('#payForm').submit();
      }
      else{
        payment.processCard();
      }
    });
  },

  processCard: function() {

    var card;
    card = {
      number: $('#card_number').val(),
      cvc: $('#card_code').val(),
      expMonth: $('#card_month').val(),
      expYear: $('#card_year').val()
    };
    return Stripe.createToken(card, payment.handleStripeResponse);
  },
  handleStripeResponse: function(status, response) {
    if (status === 200) {
      $('#payment_stripe_card_token').val(response.id)
      $('#stripe_error').remove();
      $('#payForm').submit();
    } else {
      $('#stripe_error').addClass('error').text(response.error.message);
      $('.head').css('disabled', false);
    }
  }
};
```

Generate & Migrate Payment Model
```
rails g model payment status:string amount:float email:string transaction_number:string
rake db:migrate
```

Payment Model
```ruby
# app/models/payment.rb 
class Payment < ActiveRecord::Base
  PROCESSING, FAILED, SUCCESS = 1, 2, 3
  
  attr_accessible :stripe_card_token
  
  validates :amount, :stripe_card_token, :presence => true, :numericality => { :greater_than => 0 }

  def purchase
    self.status = PROCESSING
    
    customer = Stripe::Customer.create(description:email, card: stripe_card_token)
    # OPTIONAL: save customer token for further reference
    stripe_customer_token = customer.id
    
    # Charge
    charge = Stripe::Charge.create(
     :amount => amount * 100, # $15.00 this time
     :currency => "usd",
     :customer => stripe_customer_token
    )

    if charge.paid
      self.transaction_num = charge.id
      self.status = SUCCESS
    else
      self.status = FAILED
    end
    return self
  rescue Exception => e
    errors.add :base, "There was a problem with your credit card."
    self.status = FAILED
    return self
  end
end
```

Payments Controller
```ruby
# app/controllers/payments_controller.rb 

class PaymentsController < ApplicationController
  def create
    @payment = Payment.new(params[:payment])

    if @payment.valid? && @payment.purchase
      flash[:notice] = 'Thanks for Purchase!'
      redirect_to root_url
    else
      render :action => :new
    end
  end
end
```
