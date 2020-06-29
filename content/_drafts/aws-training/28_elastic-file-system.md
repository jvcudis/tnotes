---
title: "Elastic File System"
date: 2018-11-11T12:34:51+13:00
draft: true
Categories: ["aws-training"]
---
Put the summary here
<!--more-->

Elastic File System (EFS)
- file storage service for AWS Elastic Compute Cloud (EC2) instances.
- easy to use and provides a simple interface that allows you to create and configure file systems quickly and easily
- using this makes storage capacity is elastic, growing and shrinking automatically as you add and remove files, so your applications have the storage they need and when they need it
- supports NFS version 4 (network file system) protocol
- you only pay for the storage you use, no pre-provisioning required
- can scale up to the petabytes
- can support thousands of concurrent NFS connections at once
- data is stored across multiple AZ's within a region, no durability rating yet
- block based storage and not file-based storage
- read after write consistency

> Remember how many volumes according to types you can attach to an instance. EBS can only be mounted to a single instance.

LABORATORY
1. go to the EFS page its under storage, it is only available on certain regions only
2. Create file system. configure the VPC and mount targets with default subnets and security groups.
3. add tags.
4. after being created, go to EC2 page and launch an instance with the default settings and make sure to select an AZ for your instance subnet. setup the storage and security group. create another instance. and the subnet would not be the same to the other instance.
5. provision a load balancer. and attach both the newly created 2 instances.
6. note down the ip address of the 2 instances, and make sure that the 2 instances are on the same security groups with the EFS created.
7. SSH into both instances and install apache httpd on both and start the service.
8. go to EFS and click the EC2 mount instances and there would be a command to mount EFS
9. when you try to add a webpage to one instance, there would be a copy automatically provisioned to the other instance. basically, it's allowing you to set a root directory for your files. A centralized repository of files for your EC2 instances. You can apply directory label and file label permissions too, only certain users can access certain files or directory.
10. Perfect for being a file server.
