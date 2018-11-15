---
title: "EC2 Summary"
date: 2018-11-15T14:21:12+13:00
draft: true
Categories: ["aws-training"]
---
The things you need to remember when you take the exam.
<!--more-->

TIPS

* Know the differences between the pricing models
  * On Demand - by the hour
  * Spot - bidding
  * Reserved - 12-36 month contract. upfront payment
  * Dedicated Hosts - not sharing hardware
* Know the different instance types: (FIGHTDRMCPX)
  * F1
  * I3
  * G3
  * H1
  * T2
  * D2
  * R4
  * M5
  * C5
  * P3
  * X1
* EBS consists of
  * SSD, General Purpose - GP2 - Up to 10,000 IOPS max, bootable
  * SSD, Provisioned IOPS - IO1 - more than 10,000 IOPS, faster performance, bootable
  * HDD, Throughtput Optimized - ST1 - requently accessed workloads,
  * HDD, Cold - SC1 - less frequently accessed data, file server
  * HDD, Magnetic - Standard - cheap and infrequently accessed storage, can be a bootable
  > Cannot mount 1 EBS volume to multiple EC2 instances; instead use EFS if want to share data between instances or S3 sharing
  > Termination Protection is turned off by default, you must turn it on
  > On an EBS-backed instance, the default action is for the root EBS volume to be deleted when the instance is terminated


Volumes vs Snapshots
* volumes exist on EBS: virtual hard disk
* snapshots exists on S3

* Snapshots of encrypted volumes are encrypted automatically
* volumes restored from encrypted snapshots are encrypted automatically

Snapshots of Root Device Volume

EBS vs Instance store
* you can reboot both, you will not lose your data
* by default, both root volumes will be deleted on termination. however

how to take a snapshot of a RAID array?
Problem: take

Solution: Taken an application
  * stop the application from writing to disk
  * flush all caches to the disk
  How? Freeze the file system

AMIs
AMIs are regional. You can only launch an

Monitoring
Standard: 5 minutes, Detailed: 1 minute
CloudWatch is for performance monitoring, CloudTrail is for auditing
What can i do with CloudWatch?
  - Dashboards
  - Alarms
  - Events
  - Logs

Roles
* roles are more secure way of storing your access key and secret access key on individual EC2 instances
* roles are easier to manage
* roles are assigned to an EC2 instance AFTER it has been provisioned using both the command line
*

Instance Meta-data
* Used to get information about an instance
* curl http://169.254.169.254

EFS

What is Lambda?

Placement Group.
Clustered Placement Group. Used for big data and low latency.
Spread Placement Group. important instances in different hardwares.
