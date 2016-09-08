---
layout: post
title: "Authorize Net Payment Gateway integration with rails"
date: 2011-12-29T00:36:00+05:30
comments: false
categories:
 - payment gateway
 - authorize-net
---
Authorize Net Payment gateway provides api access to enable online payments 
Gateway provides different api options to integrate - 

1. Direct Post Method
In this method gateway handles all steps required in payment transaction flow securely and clean manner. To know more on this click [here](https://developer.authorize.net/api/dpm).

2. Server Integration Method (SIM)
Here, Payment form and creditcard detail form resides on gateway site and all the steps in transaction carried out at gateway server 

3. Advance Integration Method (AIM)
Provides full control of all the transaction steps at merchant server. Payment form resides on merchant side. merchnat server sends authorization and payment capture requests to gateway server where actual transaction takes place and response is sent back to merchant server to notify transaction status. To know detail integration on this click [here](http://www.funonrails.com/2011/12/authorizenet-aim-payment-integration).

**Prerequisites before getting started with integration**

[Sign up for a test account](https://developer.authorize.net/testaccount) to obtain an API Login ID and Transaction Key. These keys will authenticate requests to the payment gateway.
