---
title: "Zipping It Properly"
date: 2018-12-07T16:23:44+13:00
draft: false
Categories: ["lambdas", "gotchas"]
---
If you have spent at least two hours of figuring out why your NodeJS lambda is throwing an error that says `Cannot find module '/var/task/index` then this is for you.
<!--more-->

Given that you hava an npm project with the following `package.json` file.

```
{
  "name": "Test",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "author": "jvcudis@gmail.com",
  "license": "ISC"
}
```

And you have a `main.js` file..

```
exports.handler = async (event) => {
  const resp = postToES(JSON.stringify(event))

  let response = {
      statusCode: 200,
      resp: resp,
      body: JSON.stringify('Hello from Lambda!'),
  };

  return response;
}
```

Then you try to create a deployment package by...

```
# Given that you are currently within the project directory
$ zip -r project.zip *
```

Once you have the zip file, you go to your AWS console and try to upload it.

By default, the lambda handler is set to `index.handler`. When you have a different filename aside from `index.js` then you have to change the default handler to that value, in this case it has to be changed to `main.handler` since our entry file is `main.js`.

Gotcha!
