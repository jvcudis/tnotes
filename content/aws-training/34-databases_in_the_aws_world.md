---
title: "Databases in the AWS World"
date: 2018-11-23T15:12:55+13:00
draft: true
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
7. Click Next to add storage and leave everything as default. Make sure to use the web security group (http and ssh access). Launch the instance and use your existing keypair if it is available.
8. To view the database connection endpoint, go to RDS -> instances and select the running rds instance. You will see the connection settings there. AWS does not give you an IP address for the db endpoint, it gives you a DNS address.
9. Next is to configure the db connection string by ssh-ing to the db instance. Do not forget to copy the connection endpoint. Open **connect.php** and replace the hostname to the copied value.
10. Access the connect.php page and it will throw an error `Unable to connect to MySQL` this is because the DB and instance does not belong to the same VPC. To make this work, go to the RDS page and find the security group settings for the db, open the inbound tab and edit the settings. Add a new rule to allow MySQL/Aurora protocols. Leave protocol, port range   with default values and set the source to use the web security group we set for the instance. Save the new rule.
11. Go back to the connect.php page and you will see a successful connection to the db.

### RDS Back Ups

AWS offers 2 types of backups:

#### Automated Backups

- allows user to recover database to any point in time within the retension period (0 - 35 days)
- takes full daily snapshot of the database and will store transaction logs throughout the day
- will be deleted when RDS instance is deleted
- enabled by default
- backup data is stored in S3 for free and it is equal to the size of the DB
- backups are taken within a defined window and backup process will affect the performance of the DB, storage I/O may be suspended and you may experience elevated latency
- when doing a recovery, AWS will chose the most rececnt daily backup and apply the transaction logs relevant to that day thus allowing users to do a point in time recovery within the retention period

#### Database Snapshots

- done manually
- stored even after you delete the original RDS instance

> IMPORTANT: When doing a db restore, either by automatic or manual way, the restored version of the database will be in a new RDS instance with a new DNS endpoint.

### RDS Encryption

Encryption at rest is supported for the following databases:

- MySQL
- Oracle
- SQL Server
- PostgreSQL
- MariaDB
- Aurora

It is done using KMS. Once your RDS is encrypted, the data stored, as well as the backups, read replicas and snapshots are also encrypted. At present, encrypting an existing DB instance is not supported. To encrypt an existing database, you must first create a snapshot, make a copy of the snapshot and encrypt the copy.

### Multi AZ RDS

Available on:

- MySQL Server
- Oracle
- SQL Server
- PostgreSQL
- MariaDB
- Aurora - by default, multi-az is turned on

Multi AZ allows you to have an exact copy of your production database in another AZ. AWS handles replication. In the event of a planned DB maintenance, DB instance failure or an AZ failure, RDS will automatically failover to the standby so that database operations can resume quickly without any intervention.

It is for disaster recovery only. Not primarily used for improving performance. Use read replicas for performance improvement.

### RDS Read Replicas

Available on:

- MySQL Server
- PostgreSQL
- MariaDB
- Aurora

Allows users to have a read-only copy of the production database. This is achieved by using Asynchronous replication from the primary RDS instance to the read replica. You use replicas primarily for very read-heavy database workloads, like on a popular wordpress website. Used for scaling and not for disaster recovery. You must have automatic backups turned on in order to deploy a read replica.

You are allowed a maximum of 5 read replica copies of any database and the read replica can have multi AZ and other region. A read replica can have it's own read replica too. Read replica will have its own DNS endpoint. You can create read replicas of Multi AZ source databases. Read replicas can be promoted to be their own databases and this would break the replication.

To enable read replicas, go to your RDS instance and in Instance actions, select create read replica.

### DynamoDB


### RedShift
