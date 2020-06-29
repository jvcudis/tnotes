---
title: "The AWS Command Line"
date: 2018-11-02T12:34:43+13:00
Categories: ["aws-training"]
draft: true
---
Put the summary here
<!--more-->

SECTION 25: Accessing Your EC2 instances

Using the AWS Command Line to Access EC2 resources.

LABORATORY
* Launch a new instance using default settings. Make sure that role is none for now. And set the security group to a default web setup.
* In IAM, create a new user and add to a group with a policy containing an Admin access or just assign an admin role to user. Copy the generated access keys for the created user.
* In your machine terminal, type in `ssh ec2-user@<ec2-ip-address> -i keypair.pem`
* Put in instructions on how to configure aws user using `aws configure`, see confluence page
* Add in the basic AWS S3 commands like aws s3 ls
* Add in instruction on how to terminate a running instance

Storing credentials locally in the EC2 instance or in your local machine is a security risk. It is also hard to manage when you have multiple instances to manage. To avoid this risk, it is advisable to use roles in accessing your AWS resources.

----

Identity Access Management and Creating Roles

LABORATORY
* Create a role in IAM and select roles to create a role. Select the AWS Service Role which has predefined selection for allowing access to AWS resources.
* Roles are created globally. Attach the S3 full access policy to the role.
* Create and launch an instance and select the created role when creating the instance that means the instance has full access to S3.
* You can now attach or replace IAM role to running instance. Before, you cannot do that so bear that in mind when taking the exam.
* SSH into ec2 instance the common way and try accessing S3. You can now access S3 without configuring credentials.
* If you check the .aws directory, it would be empty.

----

S3 CLI & Regions

LABORATORY
* Launch a new instance with default settings and don't attach any roles and web security group.
* Go to S3 and create 3 buckets in different regions. Add some objects to your buckets.
* Create a new role in IAM page. Create a full S3 access role.
* SSH into ec2 instance. When trying to do `aws s3 ls`, it would fail because the instance does not have access to S3. What needs to be done is to attach the created role to our running instance. Once that's done, you can have access to your S3 bucket.
* Try copying the bucket contents to your local file. When you copy, it would use the EC2 region by default or whatever is set on the configured region. We need to set the region flag to retrieve other buckets from other regions.

----

Bash Scripting / Using Bootstrap Scripts

LABORATORY
* create a simple webpage (make sure it's an html file).
* Go to the aws console and change region to the region you want and create a bucket where the region is the same to the region you selected.
* Upload the created webpage to the created bucket. You will get an access denied.
* Create a role with for EC2 with S3 admin access policy attached.
* Create and instance and on the configure instance, add a bash script (it's under advance details). Sample used was using `yum update`. Open the instance to the world by creating security group for HTTPS.
* SSH into instance and when you run `yum update -y` then there would be no update since it's already been run by the instance.
* install httpd and start the httpd service. check the config of the httpd service. Create a bash file where it does all the httpd installation.
* copy the content of the bucket where the webpage we created to /var/www/html by using the AWS CLI to access S3. add this step/procedure to the bash file.
* Launch a new instance and set the role to access S3 and copy the bash scripts that does all the things above where it copies the webpage to the instance and start the web server.
* In theory, the instance is now web server. if you access the instance ip address slash the webpage file then you will see the rendered page.

----

EC2 Instance Metadata

knowing how to access data of your instances or information about your instances using the terminal.

LABORATORY
* Create and launch a new instance with web security group settings.
* SSH to your newly running instance. Do not forget to elevate your to root access.
* You can curl to get the metadata of your instance `curl http://169.254.169.254/latest/meta-data/` Important thing to remember is the curl command since this would be part of the exam. this command shows you the variables you can use to get the data you want.
* If you want to get the ip-address of the instance `curl http://169.254.169.254/latest/meta-data/public-ipv4`.
* If you want to save this data to an html page `curl http://169.254.169.254/latest/meta-data/public-ipv4 > publicip.html
* You can send metadata info to s3 and save it on a bucket and use that bucket to trigger a lambda function to update things for you.

----

IMPORTANT POINTS:
- Create an AWS account and do the laboratory on my own.
