---
title: "Alias for zshrc"
date: 2019-04-05T13:35:17+13:00
Categories: ["setup"]
draft: false
---
When you get tired of typing `git status` and all the other commonly used git commands, then the thought of creating an alias for such commands would have come out of your mind. I'm using [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) and below is how I setup my git aliases on my `.zshrc` file.
<!--more-->

{{< highlight shell "linenos=table,linenostart=1,style=rainbow_dash" >}}
alias gs="git status"
alias gpush='f() { git push origin $1 };f'
alias gcheck='f() { git checkout $1 };f'
alias gcom='f() { git commit -m $1 };f'
alias gadd='f() { git add $1 };f'
{{< / highlight >}}
**LINE 2**: Notice the function assigned as alias, this code means that the alias accepts an arg `$1` and runs the function when the alias is used

Once you've saved your changes, you need to source the file so that the changes would be applied to the environment. If you want something cheeky aliases, (bullias)[checkout https://github.com/bullgit/bullias] :smiley:

![alias-on-action](/images/git-alias-on-action.gif)
