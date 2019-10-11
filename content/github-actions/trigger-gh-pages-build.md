---
title: "Trigger Github Pages Build"
date: 2019-10-08T14:37:09+13:00
Categories: ["ci", "gh-pages", "static pages", "github actions"]
draft: false
---
Recently, I've been playing with [Github Actions](https://help.github.com/en/categories/automating-your-workflow-with-github-actions) and the first thing that I've thought of doing is how I can use Github Actions aka **Steve** (let's name it Steve because Github Actions is just too unfriendly! ) to deploy my statically-generated site to Github Pages. The current deployment workflow involves TravisCI handling the generation of static files and deploying them to the `gh-pages` branch using a [built-in configuration](https://docs.travis-ci.com/user/deployment/pages/).
<!--more-->

The new planned workflow is pretty straightforward.

{{< highlight yaml "linenos=table,linenostart=1,style=rainbow_dash" >}}
name: Build and Deploy to Github Pages
on:
  push:
    branches:
    - master
    paths-ignore:
    - 'README.md'

jobs:
  ...
  deploy:
    name: Build & Deploy
    runs-on: ubuntu-latest
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      GITHUB_USER: 'Some User'
      GITHUB_EMAIL: 'someuser@gmail.com'
    steps:
    - uses: actions/checkout@master
    - name: Build static site and deploy to gh-pages
      run: |
        echo "🍙 Building static files"
        npm ci --silent
        npm run build

        echo "🍩 Deploying build directory to gh-pages branch"
        cd build
        git init
        git config user.name "${GITHUB_ACTOR}"
        git config user.email "${GITHUB_ACTOR}@users.noreply.github.com"
        git add .
        git commit --allow-empty -m 'Deploying to GitHub pages'
        git push --force --quiet "https://x-access-token:${GITHUB_TOKEN}@github.com/${GITHUB_REPOSITORY}.git" "master:gh-pages"
        rm -rf .git
    ...

[LINE 16] The GITHUB_TOKEN is provided with no additional setup but it needs to be explicitly defined if you want to use it.
[LINE 34] We push changes using the GITHUB_TOKEN as the x-access-token.
{{< / highlight >}}

Read more about [LINE 16](https://help.github.com/en/articles/virtual-environments-for-github-actions#github_token-secret) and [LINE 34](https://developer.github.com/apps/building-github-apps/authenticating-with-github-apps/#http-based-git-access-by-an-installation).

But, here's the catch! When Steve pushed changes to the `gh-pages` branch, it did not trigger a page build at all.  (-_-)ゞ゛

There is an error in the **Github Pages** settings page saying that the `Page build failed`. Honestly, it bugged me to death on why this is happening! I ran the deploy script locally just to see if the issue can be replicated but **_it works on my machine_**. But whenever I push the generated static content using the terminal, the error is gone! So it's definitely not an issue with the deploy script. ( ・◇・)？

I got frustrated and so I switched the Github Pages source from `gh-pages` to `master` and, lo and behold, it triggered a Github Pages workflow even though it was not set up and that seemed to remove the page build error. Around this time, I did not know that **when you enable GitHub Actions, GitHub installs a GitHub App** on your repository. That is the reason behind having a Github Pages workflow running whenever a page build is triggered.

It got me thinking... Σ(￣。￣ﾉ)

Maybe you need to explicitly trigger a page build!

{{< highlight yaml "linenos=table,linenostart=1,style=rainbow_dash" >}}
     ...

       git push --force --quiet "https://x-access-token:${GITHUB_TOKEN}@github.com/${GITHUB_REPOSITORY}.git" "master:gh-pages"
       rm -rf .git
       curl -H "Authorization: token ${GITHUB_TOKEN}" \
            -H Accept: application/vnd.github.v3+json" \
            -X POST https://api.github.com/repos/${GITHUB_REPOSITORY}/pages/builds

     ...
{{< / highlight >}}

So I added an API call to trigger a page build but it resulted in a message saying `Resource not accessible by integration`.

![Not Accessible](/images/github-actions-resource-not-accessible.png)

Now we are getting somewhere. The default `GITHUB_TOKEN` provided by Steve has definitely some permission limitations. Now, what do I do to have the correct permissions?  It got me into reading the different [authentication methods](https://developer.github.com/apps/migrating-oauth-apps-to-github-apps/#understand-the-different-methods-of-authentication) which resulted in me thinking that perhaps Steve (it being some sort of CI) has a server-to-server requests access but what we really want is [user-to-server requests](https://developer.github.com/apps/building-github-apps/identifying-and-authorizing-users-for-github-apps/#user-to-server-requests) which means that we need to generate an `ACCESS_TOKEN`.

{{< highlight yaml "linenos=table,linenostart=1,style=rainbow_dash" >}}
    ...

    env:
      ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      GITHUB_USER: 'Some User'
      GITHUB_EMAIL: 'someuser@gmail.com'
    steps:
    - uses: actions/checkout@master
    - name: Build static site and deploy to gh-pages

      ...

      git push --force --quiet "https://x-access-token:${GITHUB_TOKEN}@github.com/${GITHUB_REPOSITORY}.git" "master:gh-pages"
      rm -rf .git
      curl -H "Authorization: token ${ACCESS_TOKEN}" \
           -H Accept: application/vnd.github.v3+json" \
           -X POST https://api.github.com/repos/${GITHUB_REPOSITORY}/pages/builds

    ...
{{< / highlight >}}

It did solve the problem. As you can see, in the screenshot below, the Github Pages workflow has been triggered and there were no errors in the settings page.

![Queued](/images/github-actions-queued.png)

But it still does not solve the root problem of why pushing changes from my local machine works and not when Steve does it. I did a bit of googling and discovered that [GitHub Actions created by other people](https://github.com/JamesIves/github-pages-deploy-action) use the `ACCESS_TOKEN` when doing a `git push`.

{{< highlight yaml "linenos=table,linenostart=1,style=rainbow_dash" >}}
    ...

      git push --force --quiet "https://${ACCESS_TOKEN:-"x-access-token:$GITHUB_TOKEN"}@github.com/${GITHUB_REPOSITORY}.git" "master:gh-pages"
      rm -rf .git

    ...

[LINE 3] If ACCESS_TOKEN is given then use it, otherwise use the GITHUB_TOKEN.
{{< / highlight >}}

And it worked! Now, there is no need to trigger a page build using the Github API as long as you use the ACCESS_TOKEN in pushing changes.

Problem solved. Root cause identified. Now, on to more problem-solving! ＼(~o~)／
