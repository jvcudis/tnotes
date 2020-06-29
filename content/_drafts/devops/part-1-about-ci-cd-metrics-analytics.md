---
title: "Gathering CI/CD Metrics for Analytics"
date: 2019-01-30T15:49:32+13:00
Categories: ["devops"]
draft: true
---
I’ve been working around a way to gather build data from both TeamCity and Jenkins (yes! we are using two build tools, I know) and presenting them both agnostically in a dashboard of some sorts with the following metrics displayed like **Deployment Frequency** and **Build Failure Rate**.
<!--more-->

I thought, well we can send all these data to an S3 bucket and have a static app that would parse these data and plug in a visualisation library to do the pretty graphs for us. I had a plan. Nice!

:show the diagram:

So I got around doing the diagramming of how this system would work and then asked our software architect how feasible it is. It got a really good feedback in terms on scalability but the SA has got some concerns about the cost of using the S3 service and the work involved in building a static app. Each build trigger would mean a `PutObject request` and although S3 is cheap in general, it would be really costly if you send in numerous PUT requests from the lambda and the POST requests for the static app and involving more than 20 apps running multiple builds per day. And adding to the the time and energy it would take to build the static app and personally, the static app is not scalable at all. Hmmm… So change of plans!

: insert meme here:

So the plan was changed, but not drastically. Instead of using the S3 service and a static app, AWS’s Elasticsearch Service with Kibana will be used instead. It’s basically ELK stack minus Logstash! And yes to de-coupled systems!

:show new diagram here:

```
     _               _
 ___| |_ ___ _ __   / |
/ __| __/ _ \ '_ \  | |
\__ \ ||  __/ |_) | | |
|___/\__\___| .__/  |_|
            |_|
```
I started off with finding ways to send the build data to an API Gateway. TeamCity offers both webhooks and an API that you can use to grab the data you need and send it to a certain destination. I believe Jenkins also offers a plugin allowing you to send data although I have not explored it but you can always do a curl command as your post build.

```
     _               ____
 ___| |_ ___ _ __   |___ \
/ __| __/ _ \ '_ \    __) |
\__ \ ||  __/ |_) |  / __/
|___/\__\___| .__/  |_____|
            |_|
```
Once I’ve managed to send data whenever a build is completed and know what the data payload contains. I updated the API gateway and created a lambda (a dummy for now) that would allow me send back responses to the client. I don’t want the API gateway to deal with responses so I opted for a lambda proxy integration.

```
     _               _____
 ___| |_ ___ _ __   |___ /
/ __| __/ _ \ '_ \    |_ \
\__ \ ||  __/ |_) |  ___) |
|___/\__\___| .__/  |____/
            |_|
```
After that, I created the Elasticsearch domain with Kibana. There is a tutorial in the AWS blog that I followed to use Cognito as the auth service for Kibana. It is pretty straightforward.

```
     _               _  _
 ___| |_ ___ _ __   | || |
/ __| __/ _ \ '_ \  | || |_
\__ \ ||  __/ |_) | |__   _|
|___/\__\___| .__/     |_|
            |_|
```
When your ES domain is up and running, you should have access to an ES host URL. You need to update your lambda to create indexes and send it to the ES domain. You can then login on Kibana and see if ES has indeed received the logs. Before you can view the logs, you have to setup the field mappings in Kibana or you can do it programmatically.

```
     _               ____
 ___| |_ ___ _ __   | ___|
/ __| __/ _ \ '_ \  |___ \
\__ \ ||  __/ |_) |  ___) |
|___/\__\___| .__/  |____/
            |_|
```
You then setup Kibana visualisations and dashboards.

P.S.
