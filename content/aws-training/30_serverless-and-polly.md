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
2. Quick check on what the polly service offers. It's basically a text transcoder with multi-language support.
3. Create a dynamodb. Create table and set the table name and id (posts / id)
4. Create an S3 bucket. Or re-use the bucket with the already setup api gateway and route53 DNS done on the previous laboratory exercise. Leave it as a static website hosting.
5. Create a new bucket where we store the mp3 files. use the default settings.
6. Create a role that allows the lambda to talk to the needed services. Create a Lambda role and set the needed permissions. See the lambdapolicy.json file. and attache the created policy to role.
7. go back to the .com bucket and add a bucket policy. see the bucketpolicy.json file. add the policy and change the ARN value to the bucket ARN. The bucket now because public.
8. Create SNS. create a new topic.
9. Create our lambda function. python 2 will not get security patches by 2020. author from scratch. set the role to the created lambda role. see the newposts.py file. copy the code to lambda editor. pass the DB_TABLE_NAME with dynamodb table name as value and the SNS_TOPIC with the ARN value. Good practice to add a description of what the function does. Test the function by using the sample.json file. In theory, there should be data on the dynamodb after running the test.
9. Create a new lambda. Use the role created. See the convertoaudio.py file. Setup the DB_TABLE_NAME, the BUCKET_NAME is the value of the bucket where the mp3 files will be stored. don't forget to click the save button. and add a description. the polly service takes a bit of time so set the timeout to 5 minutes. Add an SNS trigger to your lambda. Select the topic that would trigger the function and save.
10. Create a new lambda that would read the posts saved at the dynamodb. See the getposts.py file. Setup the DB_TABLE_NAME env var. Add tests. `{ postIds: "*" }` this would return all the posts from dynamodb.
11. setup the api gateway. create a new api. add an api name and description. endpoint type is regional. create a GET method and choose lambda function and input the getposts lambda function name. create a new POST method and choose the lambda function and use the posts to lambda function name. Click Enable CORS and that would add Options. On the get method, enable the URL Query string parameters, add postId as query string. and then add body mapping templates and use when there are no templates defined then add application/json as content-type and then copy the mappings.json file. Deploy the API, click the '/' and deploy. check the invoke URL.
12. Go to scripts.js and change the API_ENDPOINT to the gateway invoke URL. Add the scripts.js, index.html and styles.css to the static website bucket.
13. Go back to dynamodb and delete the dummy data.
14. Test the DNS resolution.
15. Test the app. Input a text on the input box and hit the button. it would render a post id and use that post id to search for the recording.

ALEXA SKILL LABORATORY

1. go to amazon to signup for a developer account. login to your developer console so that you can test out your skill without having to publish them.
2. check the region that it is in Northern Virginia. go to lambda and go to serverless application directory and search for alexa and select the alexa fact skill.
3. you will be redirected to github.
4. name the application. this would create a cloudformation template. don't forget to delete the cloudformation template later.
5. once the lambda is up and running,
6. go to amazon the -- and create a new skill.
NOTE: Revisit this.
