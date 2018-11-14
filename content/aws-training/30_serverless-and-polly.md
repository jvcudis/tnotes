---
title: "Serverless and Polly"
date: 2018-11-12T16:21:18+13:00
draft: true
Categories: ["aws-training"]
---
Put the summary here
<!--more-->

See notebook for the drawing of how the whole this is architected.

LABORATORY

1. Encouraged to change region to Northern Virginia.
2. Quick check on what the polly service offers. It's basically a text transcoder.
3. Create a dynamodb. Create table and set the table name and id (posts / id)
4. Create an S3 bucket. Or re-use the bucket with the already setup api gateway and route53 DNS done on the previous laboratory exercise. Leave it as a static website hosting.
5. Create a new bucket where we store the mp3 files. use the default settings.
6. Create a role that allows the lambda to talk to the needed services. Create a Lambda role and set the needed permissions. See the lambdapolicy.json file.
7.
