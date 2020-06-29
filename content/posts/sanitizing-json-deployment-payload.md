---
title: "Sanitizing JSON Deployment Payload"
date: 2018-11-21T16:55:11+13:00
draft: false
Categories: ["new-relic"]
---
A few weeks ago, I was tasked to create New Relic deployment markers for our applications that have APM enabled. Just to give a background, I am pretty much new to New Relic and found the deployment markers pretty helpful when you want to know (and bl@me!) if someone has added a change that has compromised the performance of your application, 'coz it's all about traceability these days! Hurrah!
<!--more-->

There is already an exisiting solution that I can copy-paste which comes from one of the developers.

{{< gist jvcudis 085de902a13714efcd167e71f18ed93d >}}

But being a 'DevOps' person in the team, I have to ask if the deployment markers are giving them (devs! in this case) the necessary information they want. A short background, I've been doing the devops gig for approximately 4 months now and sort of learning by doing a lot of 'Google search' and seriously reading the 'Accelerate' book. I did a survey of what the developers think as the most (or more) helpful data they want to see for the deployment marker. The devs chose to see the information on the latest commit from their repository. In theory, it would have helped if the releases are tagged so in that way, we can use the tag as basis for revision. But that's another discussion with the team.

I thought it would be easy updating the code to something like:

{{< gist jvcudis 6960f1ecb76e4219d2d965a5b0710056 >}}

Here's the thing, JSON payload string interpolation is confusing. For example, we have something like `This is a small change yo` as the value of the `$GIT_COMMIT_MSG`. Apparently, the interpolation we are using which is `"'$VAR_HERE'"` only supports a single string, if it's a collection of strings (a sentence in layman's term) then it would just throw an error. So in comes JSON sanitization by using `jq`.

So below is the updated code:

{{< gist jvcudis f1cc85fe5198334e0b8574daca580752 >}}

This piece of code will run after a successful deployment. And we are using TeamCity as our tool for that.
