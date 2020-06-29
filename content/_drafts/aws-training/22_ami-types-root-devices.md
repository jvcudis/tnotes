---
title: "AMI Types Root Devices"
date: 2018-11-02T9:16:37+13:00
Categories: ["aws-training"]
draft: true
---
This section mainly talks about EC2 AMI types and the root device volumes provided by AWS. This also contains information on what is the difference between the available root device volumes -- EBS and Instance Store.
<!--more-->

### LESSON 22

AMIs (Amazon Machine Image) for your EC2 instances can be selected based on the following:

* the region and availability zone
* operating system (OS) and architecture
* launch permissions
* root device volumes

> NOTE: Root Device Volumes are volumes or disks where your operating system is running.

AWS offers its users two options to choose from for creating a root device volume for an AMI:

**EBS Volumes.** This volume is created from an **EBS (Elastic Block Storage)**[^1]. By default, AWS uses EBS volumes for root devices. Once an EC2 instance using by an AMI backed by an EBS root device volume is launched, then you cannot attach another type of root device volume to the EC2 instance[^2], in this case the Instance Store Volumes, you can attach other EBS-backed device volumes. The instance can be stopped and rebooted and not lose data

**Instance Store Volumes.** This volume is created from a template stored in S3 and used as a AMI root device for an EC2 instance. This is also known as **ephemeral storage** and takes more time to be provisioned compared to an EBS-backed AMI. AMIs using this root device volume type are **cannot be stopped** and if the underlying host fails then your data will be lost forever. EC2 instances created using this root device volume can only be rebooted and terminated. Once rebooted, you will not lose the data[^3]. They cannot be stopped and paused.


> NOTE: By default, both ROOT volumes will be deleted on termination, however with EBS device volumes, you can tell AWS to keep the root device volume.

---

##### IMPORTANT EXAM POINTS

* AWS uses the EBS as a default root device volume

[^1]: Review on EBS topic
[^2]: Need to verify if you cannot really attach an Instance Store Volume
[^3]: Quiet unclear, must review and see if it really will not lose data during reboot
