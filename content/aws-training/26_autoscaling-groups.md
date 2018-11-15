---
title: "Autoscaling Groups"
date: 2018-11-10T11:51:03+13:00
Categories: ["aws-training"]
draft: true
---
Put the summary here
<!--more-->

Launch Configurations and Auto Scaling Groups

LABORATORY

1. create your healthcheck.html file.
2. access the bucket where you placed the bootstrap index.html page. upload the healthcheck.html file to that bucket.
3. go to EC2 and make sure you create/use a load balancer where it has the correct AZ setup, the health check settings (timeout, interval, thresholds) are being set and that there are no attached instances to your load balancer.
> NOTE: Verify and test out the way how the load balancer will behave according to the set intervals and thresholds.
4. Create an autoscaling group. Before you can do that, you need to create a Launch Configurations. Just the same to creating an EC2 instance. In this case, we set the default settings and used the s3 admin role and setup the bootstrap script to setup the webserver and use the default web security group. This would allow you to download a set of keypair permissions. This is just like creating a new ec2 instance except that it does not provision an instance.
5. Now you create the autoscaling group, set the group size to 3, and use the default VPC settings. The reason behind having an autoscaling group is to have a redundant platform running on other AZ just in case the current AZ will be lost. You want to choose multiple AZ for your autoscaling group. The instance gets spread evenly among the AZ. On the advanced details, we want to enable the receive traffic from the load balancers and use the created load balancer. Healthcheck type would be ELB and the grace period is 150 seconds. Read more about the grace period. The grace period is the amount of time for the load balancer to check the instance health.
6. Set scaling policies. Select adjust to to capacity of the group. scale between 1 and 5 instances. Create an alarm for increasing and decreasing the group size. When to increase, if the avg CPU utilization >= 90% for than 5 minutes, select how many instances are added. Also can set the warm up after each step. Can also set the decreate the group according to certain conditions.
7. Configure notifications. Just use the email address.
8. Configure tags.
9. Review and the autoscaling group will be spun up. Go back to ec2 dashboard and the 3 instances (at different AZ) will be all running.
10. If you go to the load balancer page and select the load balancer, you can use the DNS name for the load balancer to load the page.
11. terminate 2 instances just for fun. ha! simulating the AZ failure. Once terminated, if you access the server using the instance ip address, the 2 addresses would fail while the load balancer DNS would not that is because traffic is still served by the other instance.
12. what the autoscaling group would do in this case is spin up a new instance for and provision it to the proper AZ. No outage at all since the traffic would be routed to other instances where the service is still running.
13. If you remove or delete the autoscaling group, it would automatically remove the instances provisioned by the autoscaling group.

----

TODO
More info about autoscaling groups
Do the laboratory exercise.
read about ecs agent.
read about what hypervisor are being used for ec2 instances
