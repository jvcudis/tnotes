---
title: "Frequently Forgotten Facts"
date: 2018-11-08T15:31:28+13:00
Categories: ["docker"]
draft: true
---

<!--more-->

docker build and run with bash access

* if your docker image has no name, it means there was an issue while building it. sometimes, when using the image name and tag when running the container, it would not locate or recognize it so you have to use the image id instead. 
