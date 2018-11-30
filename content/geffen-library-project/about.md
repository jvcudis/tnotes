---
title: "The Geffen Library Project"
date: 2018-11-15T09:57:24+13:00
draft: false
Categories: ["geffen-library-project"]
---
The Geffen Archives Project is a plan
<!--more-->

PLAN
A serverless application model.

DEVELOPMENT
1. create an S3 bucket `geffenlibrary.com` with static website hosting enabled. Add in the index.html and error.html as document names.

2. go to route53 and register a domain. the domain would be `geffenlibrary.com`. aws would create a hosted zone for your domain automatically where the information are stored about how to route traffic to your domain. if the domain will be parked, you can delete the hosted zone. continue with the process and wait for the domain to be registered. setup the alias record

3. go to apigateway.
