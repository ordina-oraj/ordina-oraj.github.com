---
layout: post
authors: [Kris_Jordens]
title: 'Introduction to API Management'
image: /img/kris_Jordens.jpg
tags: [API Management, API Gateway, Products, Responsibilities]
category: API
comments: true
---

        
# Introduction to API Management


## Table of contents
* [Introduction to API Management](#Introduction-to-API-Management)
    * [Problem description](#Problem-description)
    * [Investigation](#Investigation)
        * [API Management or API Gateway?](#API-Management-or-API-Gateway)
        * [API landscape without an API management tool](#API-landscape-without-an-API-management-tool)
        * [API landscape with an API management tool](#API-landscape-with-an-API-management-tool)
        * [Responsibilities of an API Management tool](#Responsibilities-of-an-API-Management-tool)
        * [Challenges](#Challenges)
            * [Multiple API gateways configured](#Multiple-API-gateways-configured)
            * [Clustering gateways](#Clustering-gateways)            
    * [API management products](#API-management-products) 
    * [Hands-on](#Hands-on)
        * [Installation](#Installation)
        * [Setup](#Setup)
        * [Automation](#Automation)
    * [Conclusion](#Conclusion)


## Problem description

In our current digital world where everyone is creating API’s for exposing data internally and for external partners, we should look on how we can manage and control them.
Most of us also know that we not only have to build the API, but we need to foresee additional responsibilities with it. Some that are just a necessary evil. Like for example Authorization, Authentication, or certain routing policies, etc.  
Keeping an overview on all available API's also becomes a struggle these days.
        
Well, instead of inventing the wheel over and over again, I searched for the best solution and found that an API Management tool can solve a lot of those problems and even add extra value within your landscape.
This tool helps you in managing you APIs, setting the right responsibilities on the right level. 
You can add plenty additional functionalities, but most of all this happens in one tool!
That’s one of the reasons it is important for your business to implement such a tool in your organization.
        
                
## Investigation

### API Management or API Gateway?
API management and API gateway are used interchangeably.
But is it now an API manager, API management tool, or API gateway?
Let’s dive into the naming’s.  
An API management tool refers to the application or overall solution of managing APIs.
This mostly indicates that they are talking about the application and its features.
A management tool can include one or multiple API Gateways, depending on the solution needed in the organization.
An API gateway is actually a middleware that is placed in front of your services an act as a central point to administrate, route and secure your services.


### API landscape without an API management tool
Before going into the details, let’s take a look on how most of the current API landscapes are build and how we can improve those with an API management tool.  
Without an API manager, your APIs will be the direct integration point to the business domain data.
Any integration of for example security, needs to be implemented in your API. This sometimes leads to duplicate code and the fact that your API is not only responsible for exposing the Data, but also for implementing other cross cutting functionalities.

![image](WithoutApiManagementTool2.jpg){: alt="API landscape without an API management tool" width="350" height="200" class="image fit"}
<!--![image](/img/2020-09-14-Introduction-to-API-Management/WithoutApiManagementTool2.jpg){: alt="API landscape without an API management tool" width="350" height="200" class="image fit"}-->


### API landscape with an API management tool
This all changes when we implement an API Management tool.

![image](WithApiManagementTool2.jpg){: alt="API landscape with an API management tool" width="500" height="350" class="image fit" style="vertical-align:middle;margin-left:2%"}
<!--![image](/img/2020-09-14-Introduction-to-API-Management/WithApiManagementTool2.jpg){: alt="API landscape with an API management tool" width="500" height="350" class="image fit" style="vertical-align:middle;margin-left:2%"}-->

In this situation an API management tool (API Gateway) is deployed between the client and the APIs.
All request first have to pass the API management tool, before they are forwarded to the right service.
If the Gateway is installed in the same organizational structure, it’s not required to have your API’s implementing additional cross cutting functionalities.
The API gateway will be responsible of performing the necessary checks before allowing the clients to access the Data.
I do hear you thinking already “We’re not going to install an additional tool, only for splitting out security rules from the API’s", but that’s not the only thing an API management tool can do.

### Responsibilities of an API Management tool
I listed out the most important responsibilities for you, so that you can understand the power and the actual benefits of an API Management tool:            
     
1.	Authentication 
    * Enables one of the most important parts of security on an API. Validating the person’s identity!  
    There are different kinds of Authentication:   
        * Http Basic Authentication
            * Using this approach, a user agent simply provides a username and password to prove their authentication.  
            This approach does not require cookies, session ID’s, or login pages because it leverages the HTTP header itself. While simple to use, this method of authentication is vulnerable to attacks that could capture the user’s credentials in transit.
            
        * OAuth
            * OAuth 2.0 is the best choice for identifying personal user accounts and granting proper permissions.  
            With this method, the user logs into a system. That system will then request authentication, usually in the form of a token. The user will then forward this request to an authentication server, which will either reject or allow this authentication.
        
        * OpenID Connect & OAuth 2.0 API
            * OpenID Connect is a simple identity layer on top of the OAuth 2.0 protocol, which allows computing clients to verify the identity of an end-user based on the authentication performed by an authorization server, as well as to obtain basic profile information about the end-user in an interoperable and REST-like manner.
        
        * API Keys
            * An API key is an identifier meant to identify the origin of web service requests (or similar types of requests).  
            A key is generated the first time a user attempts to gain authorized access to a system through registration. From there, the API key becomes associated with a secret token, and is submitted alongside requests going forward. When the user attempts to re-enter the system, their unique key is used to prove that they are the same user as before. This API Authentication Method is very fast and reliable but is frequently misused. More importantly, this method of authentication is not a method of authorization.   
2.	Authorization 
    * Enables fine grained authorization to API resources based on authenticated user roles.  
    This involves checking resources that the user is authorized to access or modify via defined roles.
3.  Caching resources
    * Allows caching of API responses in the Gateway to reduce overall traffic to the back-end API.  
    Latency will be improved because the API gateway can talk directly with the backend service or can use the cached resource and doesn't have to fetch the information multiple times.  
    With caching you also avoid that the backend service will be overloaded by requesting the same information repeatedly.  
    Resources can be cashed for a specified time-to-live (TTL) period.
    The gateway will retrieve the resource again from the backend service once the time has passed, and a new request comes in.
4.  White/Blacklisting possibilities
    * Allow/Block calls to specific APIs.
    * Allow/Block all calls from a given application.
    * Allow/Block requests coming from a specific IP address.
    * Allow/Block a specific user from accessing APIs.
5.  Time Restricted Access
    * Requests matching the regular expression but made outside the specified time period will receive an error code.  
    This is used to allow access to an API only during certain times.    
6.  Quota’s 
    * Rate limiting
    	* Enforces rate configurable request limits on an API.  
    	If your API becomes overloaded, its performance will suffer, and all customers will be impacted.  
        Rate limiting (also called throttling) ensures that a single user cannot intentionally or unintentionally overwhelm an API with too many requests.  
    	In case throttling kicks in, the user will receive a response status code 429 (meaning "Too Many Requests").
    	A Retry-After header might be included to this response indicating how long he/she must wait before making a new request will work again.
    * Transfer Quota
    	* Provides a way to limit the total number of bytes that can be transferred from or to an API.
7.  URL Rewriting 
    * Responses from the back-end API can be modified by fixing up any incorrect URLs found with modified ones.  
    In some cases, an API might return URLs to follow up action or data endpoints. In these cases, the back-end API will likely be configured to return a URL pointing to the unmanaged API endpoint.  
    This policy can fix up those URL references so that they point to the managed API endpoint (the API Gateway endpoint) instead. 
8.  Transformation
    * Transformation enables to convert an API format between for example from JSON and XML. 
    If an API is implemented to return XML, but a client would prefer to receive JSON data, this policy can be used to automatically convert both the request and response bodies. In this way, the client can work with JSON data even though the back-end API requires XML.
9.  Monitoring
    * Some tools let you visualize, query, route, archive, and take actions on the metrics or logs like for example the Azure API management tool.  
    Others just foresee a basic around metrics or include a third-party tool like Elasticsearch to do the job.
    
A lot more can be added or customized, depending on the tool of course. There is a wide range of products available, so please refer to the product list below if you want to know the ones used currently.  
To get hands-on experience, I mostly focused on the Open source tools like Apiman, Kong API Gateway, WSO2 API Manager.



### Challenges

There are a few things to consider before you select the right tool for your organization.  
One of the most important thing is think about the high availability of the services that you provide.  
As you might notice in previous example, if an API Gateway is placed before all your APIs it creates a SPOF (Single point of Failure).  
There are ways to handle that like setting up multiple API gateways for different endpoints or clustering your API gateways.

#### Multiple API gateways configured
![image](MultipleAPIGateways2.jpg){: alt="API landscape without an API management tool" width="500" height="350" class="image fit" style="vertical-align:middle;margin-left:2%"}
<!--![image](/img/2020-09-14-Introduction-to-API-Management/MultipleAPIGateways2.JPG){: alt="API landscape without an API management tool" width="500" height="350" class="image fit" style="vertical-align:middle;margin-left:2%"}-->

In this example you can see that 2 different Gateways are created to separate mobile from other request. The endpoint of the mobile gateway will in this case be different from the Web gateway.
If for any reason at all the Mobile API Gateway gets stuck, the client will still be able to connect to the other Gateway and request for the data he/she needs.


#### Clustering gateways
Another way to ensure high availability is for example to cluster your API Gateways.
Combined with a scalable microservice architecture, a high availability API gateway cluster ensures your application can handle large volumes of traffic and react to unexpected spikes, while being resilient to hardware failures.  
There are big differences between products when you want to cluster an API gateway.
Most products do recommend or even require to setup a Load balancer in front of the API gateway cluster. In fact, to avoid another single point of failure, it is recommended to be able to scale this load balancer as well. Or have a failover scenario in place in case the load balancer crashes. This way you can be sure that your services are always available.  
Some other products require to scale additional tools like for example Elasticsearch.  
An advantage of this solution is that the client always points to the same endpoint.

![image](HA-Gateways.JPG){: alt="High Availability API Gateways" width="600" height="350" class="image fit" style="vertical-align:middle;margin-left:2%"}
<!--![image](/img/2020-09-14-Introduction-to-API-Management/HA-Gateways.JPG){: alt="High Availability API Gateways" width="600" height="350" class="image fit" style="vertical-align:middle;margin-left:2%"}-->

This solution would be the most secure way of implementing an API gateway, ensuring that your services are available 24/7.  
Which solution to go for, is of course depending on your preferences and the level of availability that needs to be guaranteed.

## API management products

I referred to the Gartner magic quadrant to get the list of most common API management tools currently on the market.
You can find them here in alphabetical order.

![image](GartnerMagicQuadrant.JPG){: alt="Gartner Magic Quadrant" width="400" height="400" class="image fit" style="float:right    ;horzontal-align:middle;margin-left:2%;margin-right:50%;vertical-align:right"} <!--;overflow: hidden}-->

* Red Hat 3 Scale 
* Akamai API Gateway  
* Akana API Management
* API Man (open source)
* Apigee API Management
* AWS API Gateway
* Axway - AMPLIFY API Management
* Azure API Gateway
* Boomi
* Broadcom
* CA API Gateway
* Express API Gateway (open source)
* Fusio API Management (open source)
* IBM API Connect
* Kong API Gateway (open source)
* Loopback API Framework (open source)
* MuleSoft Anypoint API Management
* Oracle API Manager
* SAP API Manager
* Sensedia API Management Platform
* Sentinet 
* Software AG API Gateway
* TIBCO Mashery
* Tyk API Gateway (open source)
* WSO2 API Manager (open source)



## Hands-on
### Installation
Just to try a few things, I installed Apiman (and the required server) as a standalone solution on to my laptop and used one of my earlier created API's to test.
It was actually quite simple, however I did download a few versions of it, as some didn't work from the 1st minute.  
One of the main advantages is that it has some pre-configured settings which made it easy to start.  
Apiman is an Open Source tool, so a lot of documentation and even source code examples are available on the web.  
If you prefer, it is also possible to dockerize it and run this in a container.  
The latest version of Apiman can be found <a href="http://www.apiman.io" target="_blank" rel="noopener noreferrer">here</a>, or in case you want to try earlier versions or embedded in another server, you can also look in <a href="https://apiman.gitbooks.io/apiman-installation-guide/content/installation-guide/servlet/install.html#_installing_in_jboss_eap_7" target="_blank" rel="noopener noreferrer">this</a> link.  

Once installed, I started to play around with it.  

![image](APIMAN_Login.PNG){: alt="APIMAN Login screen" width="600" height="375" class="image fit" style="vertical-align:middle;margin-left:2%"}
<!--![image](/img/2020-09-14-Introduction-to-API-Management/APIMAN_Login.PNG){: alt="APIMAN Login screen" width="600" height="375" class="image fit" style="vertical-align:middle;margin-left:2%"}-->

### Setup
I did some research on how to set it up, and on the key aspects of Apiman.  
It's not my intention to go to much in dept of Apiman or the settings, but a few base principles I want to highlight to help you understand how easy it was to try it out and play with it.  
The first very important part in Apiman was that everything starts with an Organization.  
2ndly you need to setup a plan, which is basically a collection of policies that will be applied to requests made to Services being access through it.  
As last you need to setup a Service. 
A service contract is simply a link between an application and a service through a plan offered by that service. 
When a service contract is created, the system generates a unique API key specific to that contract. 
All requests made to the service through the API Gateway must include this API key.  

Apiman start Screen.  
![image](APIMAN_Indexpage.PNG){: alt="APIMAN start screen" width="600" height="375" class="image fit" style="vertical-align:middle;margin-left:2%"}
<!--![image](/img/2020-09-14-Introduction-to-API-Management/APIMAN_Indexpage.PNG){: alt="APIMAN start screen" width="600" height="375" class="image fit" style="vertical-align:middle;margin-left:2%"}-->

Setting up an organization (OrgHome) and creating services.

![image](APIMAN-Organization-API.jpg){: alt="APIMAN Organization and Services" width="800" height="300" class="image fit" style="vertical-align:middle;margin-left:2%"}
<!--![image](APIMAN-Organization-API.jpg){: alt="APIMAN Organization and Services" width="800" height="300" class="image fit" style="vertical-align:middle;margin-left:2%"}-->

In this example you can see a Customer API which fetches Customer information from a Spring Boot application running in IntelliJ.  
Every service can be marked public, so that everyone can access it, or you can for example secure it by creating a Client App and generate a required key via a plan.   
In next screen you can see that I created a 'SearchCustomers' client app, where the client needs to use the API-Key in the request to get the response back from our service.
Our service (Spring Boot application) didn't implement any security or authority for this.
Apiman will take the responsibility of implementing this.
Only if the client uses the right endpoint with the correct API-key, Apiman will retrieve the requested data from our backend service and send the response back to the client.

![image](APIMAN_ClientApps-APIKey.PNG){: alt="APIMAN Client apps and API-Key" width="800" height="400" class="image fit" style="vertical-align:middle;margin-left:2%"}
<!--![image](/img/2020-09-14-Introduction-to-API-Management/APIMAN_ClientApps-APIKey.PNG){: alt="APIMAN Client apps and API-Key" width="800" height="400" class="image fit" style="vertical-align:middle;margin-left:2%"}--> 

### Automation

As last I wanted to be sure that we can use the tool in an automated way, so I tried to use the Apiman REST API to create new organizations, plans and services.
I had some struggles to make it work, but eventually I was able to create everything I wanted.
Via the REST API we have the possibility to check, add or remove anything you want.
The build in Apiman-UI is also build on these REST API, so anything you can do in the application should be possible to execute via the API service.
More on this can be find in the <a href="http://www.apiman.io/latest/api-manager-restdocs.html" target="_blank" rel="noopener noreferrer">API-manager-restdocs</a>.

One sidenote, the downloaded tomcat server does not come with integrated Keycloak server, so I was not able to add users via the REST API. If you're running the standalone WildFly server, this would be integrated by default.  
I do was thinking to create a user database and let Apiman connect and check for the right credentials and user type.
But eventually to save some time, I added the users directly in Apiman to test. This is not the preferred way in a production like environment, but it did the job for now.



       
## Conclusion
There are multiple reasons why you should consider implementing an API Management tool in your organization.
Think about the benefits and segregation of duties/responsibilities of your API’s and the API management tool.  
You will be able to manage and control your APIs in one tool.
You can implement and adjust security levels like Authentication and authorization in one place.  
This tool makes it easy to add additional policies to different backend services. It can improve current latency problems and will improve the maintainability and scalability of your API landscape.  
If you are still not convinced, take the time to try out a few. 
For example, Apiman has some prebuild versions that you can download and use immediately.  
Even if you only have a few API’s running in your organization, you probably will benefit from using such a tool to manage your Security, Routing, or other Policies you want to implement.  
With an API management tool you can make sure that all your APIs just do what they need to do (sharing business related data), and that the API management tool takes care of the other cross cutting functionalities where required.  
  
  
  
##### Sources
<a href="https://www.frizztech.com/know-about-ekart-api-integration/" target="_blank" rel="noopener noreferrer">https://www.frizztech.com/know-about-ekart-api-integration/</a>.

<a href="https://koukia.ca/a-microservices-implementation-journey-part-4-9c19a16385e9" target="_blank" rel="noopener noreferrer">https://koukia.ca/a-microservices-implementation-journey-part-4-9c19a16385e9</a>.

<a href="https://www.popularowl.com/reviews/which-api-gateway/" target="_blank" rel="noopener noreferrer">https://www.popularowl.com/reviews/which-api-gateway/</a>.

<a href="https://www.okta.com/blog/2019/02/the-ultimate-authentication-playbook/" target="_blank" rel="noopener noreferrer">https://www.okta.com/blog/2019/02/the-ultimate-authentication-playbook/</a>.

<a href="https://blog.restcase.com/4-most-used-rest-api-authentication-methods/" target="_blank" rel="noopener noreferrer">https://blog.restcase.com/4-most-used-rest-api-authentication-methods/</a>.

<a href="https://apiman.gitbooks.io/apiman-user-guide/content/" target="_blank" rel="noopener noreferrer">https://apiman.gitbooks.io/apiman-user-guide/content/</a>.

<a href="https://docs.microsoft.com/de-de/dotnet/architecture/microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern" target="_blank" rel="noopener noreferrer">https://docs.microsoft.com/de-de/dotnet/architecture/microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern</a>.

<a href="https://konghq.com/learning-center/api-gateway/api-gateways-for-high-availability-clusters" target="_blank" rel="noopener noreferrer">https://konghq.com/learning-center/api-gateway/api-gateways-for-high-availability-clusters</a>.

<a href="https://docs.axway.com/bundle/APIGateway_762_AdministratorGuide_allOS_en_HTML5/page/Content/AdminGuideTopics/high_availability.htm" target="_blank" rel="noopener noreferrer">https://docs.axway.com/bundle/APIGateway_762_AdministratorGuide_allOS_en_HTML5/page/Content/AdminGuideTopics/high_availability.htm</a>.