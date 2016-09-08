---
layout: post
title: "Understanding RESTful vs SOAP web services"
date: 2014-03-17 15:09:33 +0530
published: true
comments: false
categories: REST SOAP
---
> **A Web service** is a method of communications between two electronic devices over the World Wide Web.
> It is also defined as a software system designed to support [interoperable](http://en.wikipedia.org/wiki/Interoperability) machine-to-machine interaction over a network. 

In simple words, "Web services are developed to use by other software applications"

Web services communicate via either SOAP or REST messages.
## REST
REST stands for REpresentational State Transfer. It is a term coined by Roy Fielding in his Ph.D. dissertation 
It is an architecture style for designing networked applications which typycally runs runs over HTTP.

REST is an architecture and not a "standard". There is no w3c recommendation for REST. It is concept for better web and widely accepted.

RESTful applications use HTTP requests to post data (create and/or update), read data (e.g., make queries), and delete data. Thus, REST uses HTTP for all four CRUD (Create/Read/Update/Delete) operations.

It expects client to have knowlege of what to send and what to expect in response. It is focused on accessing named resources through a single consistent interface.

#### Highlights:

1. It is simple 
2. It is lightweight and faster 
3. Platform-independent 
4. Language-independent 
5. runs over HTTP 
6. Response can be returned in "XML/JSON" format

<!--more-->
Like Web Services, REST offers no built-in security features, encryption, session management, QoS guarantees, etc. But also as with Web Services, these can be added by building on top of HTTP:

For security, username/password tokens are often used.
For encryption, REST can be used on top of HTTPS (secure sockets).

#### Example:
The Web is comprised of resources. A resource is any item of interest. For example, A software company may have many products. Clients may access particular product with this URL:

```
http://www.<company>.com/products/7
```

## SOAP
SOAP, originally defined as Simple Object Access Protocol. It is a protocol specification for exchanging structured information in the implementation of Web Services in computer networks.

SOAP is an XML based protocol for accessing Web Services. 
It is designed to handle distributed computing environments.

It is well defined mechanism for describing the interface e.g. WSDL+XSD, WS-Policy. As it is standardised it becomes complex t
It provides a large number of supporting standards for security, reliability, transactions.

Here content of the message decides what to perform and reponse (payload) must comply with SOAP schema.

#### Example

It defines 3 fundamental properties:

1. What service does: Operations provides
2. How service is accessed: Data format and protocol details
3. Where a service is located: Adress (URL) details

#### Sample SOAP request structure:
In the example below, a GetStockPrice request is sent to a server. The request has a StockName parameter, and a Price parameter that will be returned in the response. The namespace for the function is defined in "http://www.example.org/stock".
```xml
POST /InStock HTTP/1.1
Host: www.example.org
Content-Type: application/soap+xml; charset=utf-8
Content-Length: 299
SOAPAction: "http://www.w3.org/2003/05/soap-envelope"
<!- ->
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
  </soap:Header>
  <soap:Body>
    <m:GetStockPrice xmlns:m="http://www.example.org/stock">
      <m:StockName>IBM</m:StockName>
    </m:GetStockPrice>
  </soap:Body>
</soap:Envelope>
```
#### Sample SOAP response structure:
```
HTTP/1.1 200 OK
Content-Type: application/soap+xml; charset=utf-8
Content-Length: nnn

<?xml version="1.0"?>
<soap:Envelope
xmlns:soap="http://www.w3.org/2001/12/soap-envelope"
soap:encodingStyle="http://www.w3.org/2001/12/soap-encoding">

<soap:Body xmlns:m="http://www.example.org/stock">
  <m:GetStockPriceResponse>
    <m:Price>34.5</m:Price>
  </m:GetStockPriceResponse>
</soap:Body>

</soap:Envelope>
```
#### Highlights 
Here are few **reasons** for which you may want to use SOAP: 

1. Platform and laguage independent  
2. Follows W3C standard  
WSDL (Web services description language) is a W3C standard based on xml and is used to describe web services.
3. Security  
It supports SSL (just like REST). It also provides a standard implementation of data integrity and data privacy. Being "Enterprise" it's more secure.  
4. Atomic Transactions  
It supports ACID Transactions (WS- Atomic Transactions) so it can provide two phase commit over distributed transactional resources.  
5. Reliable Messaging   
It has successful/retry logic built in and provides end-to-end reliability even through SOAP intermediaries 

Whenever complex or likely to change API getting developed then SOAP could be a good choice.

#### References used
1. http://rest.elkstein.org/
2. http://www.xfront.com/REST-Web-Services.html
3. http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm
4. http://www.w3schools.com/webservices/ws_soap_example.asp
5. http://www.slideshare.net/kverbert/soapbased-web-services
