---
layout: post
title: "Give your codebases a parent directory"
date: 2013-12-06 21:07
comments: false
categories: feed geek
published: true
external-url:
---
 A quick tip for developers who keep all their code repositories directly under one directory (like I used to)
<!--more-->
I've always been somewhat fascinated by the organizational tricks and mastery of others, so I figured it worthy to share one that occurred to me.

Let's say your structure looks like this

```
dev
 - proj_a
  - index.html
 - proj_b
  - index.html
 - proj_c
   - index.html
```

This is fine and what I was doing for a while. And it works well enough. But over time I've acquired a fair deal of images, database dumps and notes, and they're mostly scattered about in ~/Downloads. So, instead do something like this

```
dev
  - proj_a
    - database_dump.sql
    - todo.txt
    - code
      - index.html
  - proj_a
    - code
      - index.html
 - proj_c
   - index.html
```

You'll note the last project is unchanged; not every project will need this structuring. This change is nothing revolutionary, but its useful enough to mention and has alluded me for a while. Now everything related to that project has a place to live.

**TL;DR** I should have done this a while ago, and this is not how you use tl;dr