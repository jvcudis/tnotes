---
title: "Using CloudWatch for EC2 Monitoring"
date: 2018-11-02T11:30:00+13:00
Categories: ["aws-training"]
draft: true
---
This section talks about EC2 Monitoring -- the default monitoring and monitoring thru CloudWatch. This also talks about CloudWatch offerings and what each CloudWatch feature is can do.
<!--more-->

### LESSON 24

EC2 has basic monitoring enabled by default which monitors the instances every **5 minutes**. You can find this by selecting the instances, selecting a specific instance and then there would be a Monitoring tab showing the basic instance metrics.[^1] To have a detailed monitoring for your instances, you have to do it on CloudWatch[^2].

CloudWatch allows you to create the following:

**Dashboard.** Create dashboards that shows what is happening to your AWS resources. It allows you to create widgets to be added to your dashboard. The text widget is used to add labels and text to your dashboard. You can use the markdown language for text widgets. The graph widget allows you to create graphs by choosing from predefined metrics. You can select a resource (EC2 instance, S3, etc.) and the default metrics will be shown for you to select.

**Alarms.** Allows you to set alarms that notify when a particular thresholds are hit. For example, you want to setup an alarm for you EC2 instance whenever the CPU utilization exceeds 80%. What you have to do is create an alarm, selet the metric we want, in this case the CPU utilization metric and then define the alarm and setup the notification.[^3] The alarm created will be shown on the alarms page once created.


**Events.** Helps respond to state changes in your AWS resources.[^4]


**Logs.** Allows you to aggregate, monitor and store logs coming from your instances. To enable logging for your instances, an agent must be installed on your EC2 instance to be able for it to send CloudWatch logs.[^5]


**Metrics.** Instead of creating a dashboard, you will see all available metrics for all your AWS resources. By clicking the AWS resource and the metric, a graph will be shown on the page. A good way to know what are the availabe metrics for a specific resource.

---

##### IMPORTANT EXAM POINTS

* Standard monitoring is **5 mins** while detailed monitoring is **1 min**
* Dashboards allows you to see your AWS environment as a whole
* If no metrics are available, you can create your own metrics[^6]
* Alarms allows you to set alarms that notify when a particular thresholds are hit
* Events helps respond to state changes in your AWS resources
* Logs allows aggregate, monitor and store logs
* Metrics is an easy way of knowing the performance of your AWS resources
* CloudTrail vs CloudWatch. CloudTrail is for auditing, it monitors your AWS account activities while CloudWatch is for logging, it monitors the resources and performance of your AWS environment.

[^1]: Test and confirm this. Have a screenshot showing where the Monitoring tab is.
[^2]: Verify if CloudWatch is global or region-based.
[^3]: Create a GIF showing how to create an alarm.
[^4]: State a specific example of how CloudWatch events works.
[^5]: How to install the agent? More of a Develor Associate and not a Solutions Architect Associate exam question.
[^6]: Custom metrics creation?
