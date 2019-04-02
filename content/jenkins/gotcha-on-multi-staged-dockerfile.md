---
title: "Jenkins Declarative Pipelines Gotcha When Using Multi-Staged Dockerfile"
date: 2019-01-31T12:59:49+13:00
draft: false
Categories: ["jenkins", "dotnet", "ci"]
---
When I tried using the Jenkins **Declarative Pipeline** to build, test and package a .NET Core lambda within a container using a multi-staged dockerfile, it throws an error saying `Cannot retrieve .Id from 'docker inspect <x> as <y>'`. The strange thing is that it *did* build the different stages successfully. I checked by running `docker images` to see what images are currently available in the system. I did a bit of research and found out that it is an existing issue of the Jenkins docker plugin [issue](https://issues.jenkins-ci.org/browse/JENKINS-44609).
<!--more-->

Below is the Jenkinsfile and Dockerfile that throws the error:

`Dockerfile.build`
{{< highlight Dockerfile "linenos=table,linenostart=1,style=rainbow_dash" >}}
FROM microsoft/dotnet:2.1.503-sdk-alpine as appbuild

# Copy csproj and code and restore as distinct layers
WORKDIR /app
COPY /src ./
RUN dotnet restore

# Copy the unit tests code and run the tests
FROM appbuild as unittestrunner
WORKDIR /tests
COPY unittests ./
ENTRYPOINT [ "dotnet", "test" ]

# After tests pass, publish app with library deps, and package the lambda
FROM appbuild as lambdapackager
WORKDIR /package
COPY --from=appbuild app/ ./
RUN apk update && apk add zip && \
    dotnet publish -c Release
ENTRYPOINT [ "dotnet", "lambda", "package" ]
{{< / highlight >}}

`Jenkinsfile`
{{< highlight groovy "linenos=table,linenostart=1,style=rainbow_dash" >}}
pipeline {

  ...

  stages {
    stage('Build') {
      steps {
        script {
          docker.build('app', '-f Dockerfile.build .')
        }
      }
    }

    ...

  }
}
{{< / highlight >}}

I tried the workaround of removing the stage names and instead use the build number but it still throws the same error. I opted on not using the plugin and updated my Jenkinsfile.

`Jenkinsfile`
{{< highlight groovy "linenos=table,linenostart=1,style=rainbow_dash" >}}
pipeline {

  ...

  stages {
    stage('Build') {
      steps {
        script {
          sh "docker build -t app -f Dockerfile.build ."
        }
      }
    }

    stage('Run Unit Tests') {
      steps {
        script {
          sh "docker build --pull --target unittestrunner -t app:unittest -f Dockerfile.build ."
          sh "docker run app:unittest"
        }
      }
    }

    stage('Package Lambda') {
      steps {
        script {
          sh "mkdir output"
          sh "docker build --pull --target lambdapackager -t app:packager -f Dockerfile.build ."
          sh "docker run --rm -v '${PWD}'/output:/package/bin/Release/netcoreapp2.0/ app:packager"
        }
      }
    }

    ...

  }
}
{{< / highlight >}}

Everything now works! Except for the failing tests! Ha!
