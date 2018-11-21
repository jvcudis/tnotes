---
title: "Route 53"
date: 2018-11-16T15:03:23+13:00
draft: true
Categories: ["aws-training"]
---
A laboratory.
<!--more-->

1. Go to Route53 and register a domain name. Route53 is a global service.
2. Input the domain name you want and continue with filling in the contact details. And continue with the additional steps.
3. Wait for the domain name to be registered.
4. Once it is registered, you can go to hosted zones and click on the domain name, it would show you the NS record and the SOA.
5. You would see that AWS gives you 4 NS record values. It is because when one of them is down, it would still point to the other ones.
6. Go to EC2 and create 2 instances in one region. Do it again for another 2 regions except have 1 for each region. Total of 4 instances. Make sure that there is a bootstrap script that makes an instance a web server. This instances will be used for testing.
6. Create a record set. See below routing policies.

Routing Policies that AWS Offers when you create a record set
- simple routing
  - default policy when creating a new record set. used commonly when you have a single resource. one web server. see notebook for diagram.
  - laboratory. open route53 and go to the DNS record. create a new record set. created a naked domain name which is an alias record. there's nothing in front of your domain name. choose Yes for Alias. For alias target, select a load balancer or any resource. How it works? Whenever you go to the domain name, example.com then it would redirect to the DNS name of the load balancer. load balancers are not given ip addressses, only DNS names

- weighted routing
  - certain percentage of the traffic goes to a resource while the other percentage goes to another resource. split traffic according to weights assigned. see notebook for diagram.
  - laboratory. open route53. click on the hosted zones and go to the DNS record. delete the old alias record set and create a new record set. you need to grab the IP address of a region and AZ and add it on the Value and set the TTL to 60seconds. Change the routing policy to weighted and setup the weights. weight is 25%. Create another record set using the other region and AZ and set the weight to 25. The weight value is a percentage and is computed according to the total inputted percentages.

- latency-based routing
  - sends the traffic to the server where the lowest latency is. see notebook diagram.
  - laboratory. go to route53 and go to the DNS record. create a record set and input a valid ip address and choose latency as policy and it would not create it since there are existing weighted alias records created before. those records needs to be deleted. you can't mix and match different routing policies. create a new record using naked domain and 60secs as TTL. Place the ip address of two instance (region 1, diff az) and select latency as policy. create another record set using the ip address/es of instance from different region. create another record for another instance ip address from the other region. the region would be automatically selected under the routing policy settings.

- failover routing
  - used when you want to create an active/passive setup. primary site in one region and the secondary DL site in another region. route53 will monitor the health of primary site using healthcheck whick monitor the health of the endpoints. see notebook for diagram.
  - laboratory. go to route53 DNS records and delete the existing A records. Create a healthcheck under health checks and choose endpoint and choose IP address and input the ip address, hostname, port and index.html file. wait for healthcheck to be online. go back to domain name list and create a new record set using failover policy and setup by inputting the ip address and selecting primary and the created healthcheck. create another record set using failover policy using an ip address from another instance in diff region and use failover with secondary with no healthcheck. change ttl to 60 for faster failover. how it works? stop the instances pointed by the primary and see the healthchecks are unhealthy and you can't access the site. when you refresh again, it would point to the secondary server.

- geolocation
  -

NOTES:
- what is a hosted zone?
- read faqs for each routing policy
- can i used the different routing policies for an S3 static website resource?
