---
title: "Lambdas"
date: 2018-11-11T13:36:28+13:00
draft: true
Categories: ["aws-training"]
---
Put the summary here
<!--more-->

A brief history of cloud

data center -> iaas -> paas -> containers -> serverless

What is a lambda? Read AWS docs.
- a compute service where you can upload your code and create a lambda function. AWS lambda takes care of the provisioning and managing the servers that you use to run the code. You don't have to worry about operating systems, patching, scaling, etc. You can use Lambda in the following ways.
: as an event-driven compute service where AWS lambda runs your code in response to events. these events could be changes to data in an Amazon S3 bucket or an amazon dynamodb table
: as a compute service to run your code in response to HTTP requests using amazon api gateway or api calls made using aws sdks. this is what is used a acloudguru
: lambda scales really well, you don't have to deal with load balancing. it scales out (know difference between scale up, scale down, scale out)

Lambda Triggers
1. go to lambda page and create a function. 3 ways of authoring a function. choose from scratch and create a new role from template. use simple microservice permissions.
2. add a trigger. and remember the major triggers (api gateway, sns, s3, etc). need to be familiar with the common triggers when going to the exam.
3. everytime a user sends a request to api gateway, it will be served by a one lambda function, when two users then 2 lambdas will be triggered.
3. supported languages are: c#, python, nodejs, go, java

how lambda is priced. disruptive costs. paying only when it is run.
number of requests: first 1M requests are free. 0.20 per/1M requests thereafter
duration: duration is calculated from the time your code begins executing until it returns or otherwise terminates, rounded to the nearest 100ms. the price depends on the amount of memory you allocate to your function. You are charged 0.00001667 for every GB-second used. max threshold for functions is 5 mins(15 mins currently).

Pros for lambdas
- no servers!
- continuous scaling
- really cheap

use case
- everytime you speak to alexa, that is a lambda being triggered

TIPS
- lambda scales out (not up) automatically
- lambda functions are independent, 1 event = 1 function
- lambda is serverless
- know what services are serverless, it is important for exam
- lambda functions can trigger other lambda functions, 1 event can = x functions if functions trigger other functions
- architectures can get extremely complicated, AWS x-ray allows you to debug what is happening
- lambda can do things globally, you can use it to back up s3 buckets to other s3 buckets
- know the triggers available for lambdas

---

LABORATORY: Build a serverless webpage with route53, lambda, api gateway and s3

how it should work: see drawing on notebook.

1. create an s3 bucket. the bucket name and domain name must be the same. so make sure that the bucket is available before registering a domain name. enable static website hosting for the bucket. make sure to set the index.html and error.html.
2. register a domain name on route53. it costs 12 dollars. make sure it is the same name to the created S3 bucket.
3. configure lambda function. create a function and use author from scratch. (from scratch, options are blueprints, serverless application repository)
4. for this, use python 3.6 and create new role for lambda. for policy templates, use the microservice policy.
5. copy the given lambda sample to your newly created lambda and save the changes.
6. add the triggers. (memorize the lambda triggers because it will be asked what are the valid lambda triggers). select API Gateway as a lambda trigger and then configure it.
7. Choose Create a new API and input the api name. deployment stage is prod and for security, select open. (understand what each security option does.) and then add.
8. the invoke url will be generated. click on the api gateway and it will bring you to the api gateway page.
9. delete the default method and create a new GET method. choose lambda function as integration type and choose use lambda proxy integration. take note of where you lambda region is and select it and input in the lambda function name. check the use default timeout.
10. go to actions and click deploy API.
11. when you want to know where the invoke URL is, go to stages -> prod -> GET method and you will see the invoke URL. when clicked, it would run the lambda.
12. test out your invoke URL by doing a curl call. go to your index.html and put in the real API Gateway invoke URL.
13. go back to s3 page and upload the index.html and error.html files and make them both public.
14. go to route53 and go to hosted zones and create a recordset. this will be an alias record and select the s3 website as target.
15. open the browser and type in your domain. ho and behold, you got a serverless website!
