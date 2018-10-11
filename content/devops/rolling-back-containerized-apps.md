---
title: "Rolling Back Containerized Apps"
date: 2018-10-11T21:22:51+13:00
draft: true
Categories: ["devops"]
---

Since rolling back is not part of the everyday build process, it is something often left neglected. You only need it for emergency purposes. It's like one of those fire extinguishers in your office, you know it's there, you have no idea how to use it and it will save your life when there is a fire. Yep! That is what deployment rollback procedures are.

I was tasked to setup a rollback procedure for the team. First thing to know when you want to know how to rollback is how a certain application is deployed. In this case, we have an app that runs within a container and deployed to ECS. We use TeamCity for our CI/CD tool and push our images to ECR. We also design our builds to always deploy to production.

<diagram of the deployment  here>

Give the manual ways of doing a rollback.

How to automate it. There are tagging models. Once is through the build version, the other one is through the commit hash. Personally, I prefer the build version model since containers are deliverables that are not dependent to other packages and to know the latest commit hash used by the container, then that could easily be viewed on TeamCity. And we don't do release tagging for our repos (which is unfortunate. T_T).

<diagram showing the build steps for adding tags>

Some teams heavily rely on repository for releasing. Build versions are not given that much importance and would rather know the commit hash or the repo tags.

<updated diagram showing the build steps for adding tags using commit hash>

How to create a deployment rollback pipeline.

<diagram for deployment rollback pipeline>

Verifying rollbacks are tricky too. We have some apps that has a custom endpoint where it returns the current version of the app. And since rolling back means not rebuilding but instead swapping the container with a version that was previously working then you cannot be sure that your tests would not avail against the rolled back app. I have not the vaguest idea how to automate the testing of a rolled back app.

So far, the opportunity to use the deployment rollback procedure has not come yet. And there is still the discussion on rolling back and rolling forward.
