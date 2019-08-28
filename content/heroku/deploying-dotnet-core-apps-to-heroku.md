---
title: "Deploying Dotnet Core Apps to Heroku"
date: 2019-08-28T14:59:38+12:00
draft: false
Categories: ["heroku"]
---
The team I was working with has a test that has been failing for months because said test is looking for a service that does not exist in the test environment it is running on. In local development, the developers have setup mock services for them but they have not setup anything that can be consumed by the app deployed in the test environment. It was a bit disturbing for me seeing a test failing for months without having any idea why it was like that or why it is left in that state.
<!--more-->

For somebody that is not familiar with the source code and just came upon the project to implement a version-controlled deployment workflow, it was alarming because I thought it was me who broke the tests. Personally, I would prefer for projects to be more transparent of its current state. One way to achieve that is to write something in the README file about the broken test, skipping the test could also be an option or just fix it. Anyways... Long story short, I deployed the mock service to Heroku to satisfy the failing-tests-hater self.

So everything is all good and green... NOT! :smile: Apparently, it's a big security issue! I have an inkling that deploying to Heroku could be against the laws that the security team abides to but faster deployment means faster results so I went ahead with it. It actually took a week for the security team to figure that out. Ha! I'm still not sure if they found out because somebody reported it or because they have a mechanism for knowing such things. **It would be cool to have something like a bot or a Githup App reporting security issues to developers.**

If you plan on using Heroku, the documentation on [Docker Deploys](https://devcenter.heroku.com/articles/container-registry-and-runtime) is a good starting point. The most important thing to remember is that Heroku allocates the port for your app dynamically so that means you have to use the `$PORT` environment variable.

{{< highlight nginx "linenos=table,linenostart=1,style=rainbow_dash" >}}
FROM mcr.microsoft.com/dotnet/core/sdk:2.2
<do app things here>
CMD ASPNETCORE_URLS=http://0.0.0.0:$PORT dotnet run --project App.csproj
{{< / highlight >}}

> In a Node app, the `$PORT` is binded like so `CMD npm start --bind 0.0.0.0:$PORT`

Given that we have created a Heroku app named 'mighty-river', we can push the container to the Heroku docker registry. Make sure to build and run the container locally first before pushing it. Initially, I had problems pushing the container because Heroku could not find the app but I found out that you can use the `-a` parameter to specify the app name but it was tricky figuring that out because it was not really that visible in the documentation. Once pushed, you can release the app and everything should be good.

{{< highlight bash "linenos=table,linenostart=1,style=rainbow_dash" >}}
$ heroku container:push web -a mighty-river
$ heroku container:release web -a mighty-river
{{< / highlight >}}

Overall, Heroku is pretty easy to use. I would like better documentation and more examples on how to deploy different dockerized applications using different technology stack.
