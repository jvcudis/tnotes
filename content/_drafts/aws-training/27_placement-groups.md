---
title: "Placement Groups"
date: 2018-11-11T12:21:38+13:00
draft: true
Categories: ["aws-training"]
---
Put the summary here
<!--more-->

Placement Groups

what are placement groups? Read AWS doc.

There are 2 types of placement groups.

* Clustered Placement Groups
  - mostly talked about exam
  - grouping of instances within a single AZ
  - recommended for applications that need low network latency, high network throughput or both
  - only cetain instances can be launched in to a clustered placemnet groups, cannot launch nano and micro. see what are the instances supported by placement groups
  - oldest type of placement group
  - if you want your data in one AZ then use this placement group

* Spread Placement Group
  - new type of placement groups
  - group of instances that are each placed in dstinct underlying hardware
  - recommended for application that have a small number of critical instances that should be kept separate from each other
  - opposite of the clustered placement group
  - instances are spread across multiple AZ

NOTES

* ckustered placement group can't span multiple AZ while spread placement can
* the name you specify for a placement group must be unique within your aws account
* only certain types of instances can be launched in a placement group (compute optimized, GPU, memory optimized, storage optimized)
* AWS recomemend homogenous instances within placement groups, which means that instance size and family must be the same
* you can't merge placement Groups
* you can't move an existing instance into a placement group. what you can do is create an AMI from your existing instance, and then launch a new instance from the AMI into a placement group
