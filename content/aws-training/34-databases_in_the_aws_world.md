---
title: "Databases in the AWS World"
date: 2018-11-23T15:12:55+13:00
draft: false
Categories: ["aws-training"]
---
This talks about the basic database concepts, like what a relational database is. If you are a database admin and have a clear grasp about database management then this section could be skipped. This also tackles the different database solution offerings from AWS aside from the **Relational Database Service (RDS)**, which are DynamoDB, RedShift, Elasticache, and Aurora.
<!--more-->

<hr>

### LESSON 34

### AWS Database Types

* RDS - OLTP or Relational Database
* DynamoDB - NoSQL
* RedShift - OLAP or Data Warehousing
* Elasticache - In Memory Caching

### Relational Database Concept

Please read AWS's post on Relational Database: https://aws.amazon.com/relational-database/. Below are different relational database types available:

  - SQL Server - Microsoft product, paid
  - Oracle - mostly used for big enterprises
  - MySQL Server - opensource offering, free
  - PostgreSQL - opensource
  - Aurora - AWS's offering, comes in 2 flavors which is MySQL and PostgreSQL. AWS offers migration from PostgreSQL/MySQL to Aurora
  - MariaDB - also one of the popular options

### Non Relational Databases (NoSQL)

JSON format is used by NoSQL. It allows for nested values. **DynamoDB** is the AWS offering.

### OLTP vs OLAP

The difference would be the types of queries to run.

#### OLTP (Online Transaction Processing)
Example, you got an order number 2120121 and OLTP pulls up a row of data that contains data such as name, address, etc.

#### OLAP (Online Analytics Processing)
Used for data warehousing. Example would be running a query to get the number of products sold in a certain region or continent or get the net profit of a product. The query would pull in large number of records and do calculations to process the request.

### Data Warehousing

* Used for BI (Business Intelligence) and some of the BI tools are Cognos, Jaspersoft, SQL Server Reporting Services, Oracle Hyperion, SAP NetWeaver
* Used to pull in very large  and complex data sets
* Used usually by management to do queries on data (such as current perf vs targets)
* Database warehouse databases use different type of architecture both from a database perspective and infrastructure layer

### What is Elasticache

It is a web service that makes it easy to deploy, operate and scale an in-memory cache in the cloud. It improves the performance of the web applications by allowing you to retrieve information from fast, managed, in-memory caches instead of relying on a slower dis-based databases. It supports both **Memcached** and **Redis**. For example, you display the top products in your page. What mostly happens is that when someone accesses the page, the app would query the data from the database. This kind of data might not be changed or updated often, so to have a more optimized service, this query or data can me cached for a certain amount of time. Having that data cached could free up the database to doing more valuable processes.

<hr>

### RDS Laboratory

#### Create and connect to AWS RDS Service
1. Go to RDS page, and **Launch a DB instance** and select MySQL. NOTE: Can't use Aurora on free-tier. Go to **Next** and leave everything as default.
2. Setup the _DB instance identifier_, the _Master username_ and _Master password_. The instance identifier is unique for all DB instances in you AWS account in the current region so this is your DB instance name.
3. Next would be to configure the advanced settings. Use the default VPC and subnet group. Make sure that your RDS instance it is not publicly accessible. Make sure that you _Create a new VPC security group_ for your RDS. Setup the _Database name_ and leave everything by default.
4. Launch the DB instance. It could take a few minutes to provision the instance.
5. Go to the EC2 page and create an instance. Use the Amazon AMI t2.micro and leave everything as default except for the bootstrap script which is under _Advanced Details_.
6. Use the below bootstrap script. This script would setup an Apache server, install php and php-mysql to your instance. It will also create an index page containing the php info and download a script from S3. Make sure you have the **connect.php** script on your own S3 bucket.
{{< gist jvcudis d34b7d6c7ff909dbe94ca979329a4eef >}}
7. To be continued...
