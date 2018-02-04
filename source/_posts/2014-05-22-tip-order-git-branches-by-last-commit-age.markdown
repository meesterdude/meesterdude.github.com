---
layout: post
title: "Tip: order git branches by last commit age"
date: 2014-05-22 08:24
comments: false
categories: feed geek
published: true
external-url:
---
{% img center /images/sorted_git_branches.png  git branches by age %}

A great tool for git users

<!--more-->

To try this out on your own, put this in your terminal:

```
for k in `git branch|sed s/^..//`;do echo -e `git log -1 --pretty=format:"%Cgreen%ci %Cblue%cr%Creset" "$k"`\\t"$k";done|sort
```

or create an alias for it in your `~/.bash_profile`

```
alias git-branch-list='for k in `git branch|sed s/^..//`;do echo -e `git log -1 --pretty=format:"%Cgreen%ci %Cblue%cr%Creset" "$k"`\\t"$k";done|sort'
```

See the post for this trick [here](http://www.commandlinefu.com/commands/view/2345/show-git-branches-by-date-useful-for-showing-active-branches). You can find other cool tricks on [commandlinefu](http://www.commandlinefu.com/).