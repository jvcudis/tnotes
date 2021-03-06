---
title: "AWS Load Balancers"
date: 2018-11-02T10:41:49+13:00
Categories: ["aws-training"]
draft: true
---
This section will talk about the basics of load balancers, the different types of load balancers AWS currently is offering, how to create them and associate them to an instance and some important points that is commonly asked in the AWS Solutions Architect Assosiate Certification Exam.
<!--more-->

### LESSON 23

**What is a load balancer?** A VM that balances the load across your web servers. Duh!

There are 3 types of AWS load balancers:

**Application Load Balancer.** Best suited for load balancing HTTP/HTTPS traffic and operates at OSI Layer 7. This type of load balancer is application-aware[^1] which means that they are intelligent and can create advanced request routing where you can send specified request to specific web servers.

**Network Load Balancer.** Used for load balancer **TCP traffic where extreme performance is required**. They operate at the connection[^2] level or the OSI Layer 4. It is capable of handling millions of requests per second while maintaining extra-low latencies.

**Classic Load Balancer.** Also called **Elastic Load Balancer (ELB)**. Load balances HTTP/HTTPS apps and also uses OSI Layer 7 specific features such as X-Forwarded-For headers and sticky sessions[^3]. It mostly operates at OSI Layer 4 and use strict load balancing for apps that rely purely on the TCP protocol.


#### Errors

When the app stops responding, the Classic Load Balancer (ELB)[^4] responds with a 504 error (timeout error). This means that the app is not responding within the idle timeout period and so we need to troubleshoot and check if there are issues with the web server or the database

#### X-Forwarded-For

The Classic Load Balancer (ELB) has an internal IP address which is used by the EC2 instances that are associated to it. This internal IP address is the only address the EC2 instance can capture and use. Sometimes, we want to know where the request originates from and thus we want to know the user IP address, this information is saved on the X-Forwarded-For header.

```
       :smiley_cat:                                     :cop:                                     :computer:
      USER       ------------------>    LOAD BALANCER    ------------------>    EC2 INSTANCE
   145.67.8.1                             10.0.0.3                               10.0.0.3
                                                                        X-Forwarded-For: 145.67.8.1
```

### Load Balancers - LAB

#### SETTING UP A HEALTHCHECK URL ON YOU EC2 INSTANCE
1. Go to EC2 page and fire an existing instance or create a new one
2. SSH into the instance and check that the httpd service is running[^5]
3. Go to the /var/html/www directory and create a `healthcheck.html` file

#### CREATING A CLASSIC LOAD BALANCER (ELB)
1. Go to EC2 page and select Load Balancers
2. Create a Classic Load Balancer (ELB)
3. Follow the instructions on how to create[^6]
4. Configure health check settings[^7]
5. Setup tags because they can help with cost optimization
6. Once all done, refresh the page and you should see that the load balancer will report that the instance is now **InService**

#### CREATING AN APPLICATION LOAD BALANCER
1. Go to EC2 page and select Load Balancers
2. Create an Application Load Balancer
3. Follow the instructions on how to create[^8] a load balancer, should select internet-facing
4. Configure routing and healthcheck settings, you can also specify success codes (200, etc) as response
5. Register target groups and assign EC2 instance
6. Once all done, refresh the page and you should see that the load balancer will report that the instance is now **InService**

#### SIMULATING A LOAD BALANCER ERROR
1. Go to EC2 page and fire an existing instance or create a new one
2. Delete the healthcheck.html page
3. The load balancer will report that the instance is **OutofService**

> NOTES:
>
> * Classic Load Balancers (ELB) and Application Load Balancers are given DNS name generated by AWS and not an IP address
> * Network Load Balancers are beyond the scope of the SAA exam.

---

##### IMPORTANT EXAM POINTS

 * Remember that types of load balancers and what there functionalities are
 * 504 errors means that the gateway has timed out and there could possibly be an issue with the app which means there is a need to troubleshoot the web application or the database
 * To know the user IPv4 address, look for the X-Forwarded-For header
 * EC2 instances monitored by the load balancers can have either a be InService or OutofService
 * Health checks for instances are done by talking to a specific URL specifically used for health checks
 * Load balancers have their own DNS name and not given an IP address

[^1]: Research on application-awareness
[^2]: Officially called Transport Layer on the OSI model
[^3]: What are sticky sessions
[^4]: Verify if both Application and Network Load Balancers behaves the same way
[^5]: Learn more about httpd
[^6]: Setup own Classic Load Balancer
[^7]: Understand the health check settings
[^8]: Setup own Application Load Balancer
