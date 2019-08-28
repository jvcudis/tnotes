---
title: "Loading Environment Variables"
date: 2019-04-05T14:52:28+13:00
Categories: ["jenkins"]
draft: true
---
A CI/CD tool's workflow or actions will most likely depend on a value that the tool will get from an environment variable setting. In Jenkins, there is the environment wrapper where all key-value pairs you add within it gets exported. That wrapper can be used within the pipeline and also within the stages. It is a really neat feature. But what if you have to export all these environment variables to multiple Jenkinsfile?
<!--more-->

This is where it gets tricky because when you want to update a default value of an variable then you have to do an update on multiple Jenkinsfile. Not ideal at all. One way of solving this problem is to make sure that the environment variables file exists in a single space and when it is updated then the Jenkins should pick up these updates with no changes on the Jenkinsfile.

Jenkins has this other neat ability of defining a (shared library)[https://jenkins.io/doc/book/pipeline/shared-libraries/] that could help. How this is done then?


{{< highlight groovy "linenos=table,linenostart=1,style=rainbow_dash" >}}
TEST_ON=true
{{< / highlight >}}

Let's write some test first

{{< highlight groovy "linenos=table,linenostart=1,style=rainbow_dash" >}}
TEST_ON=true
{{< / highlight >}}
