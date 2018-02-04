---
layout: post
title: "11 lessons from a codebase"
date: 2014-12-29 04:46
comments: false
categories: feed
published: true
external-url:
---

I've been fixing up the codebase of a rails app for a few months now. It was written by some smart folks, so I wanted to share some of the philosophies and design patterns they utilized.

<!--more-->

1. You should **always** write tests. The only time its ok to not write tests is when its for public facing, non-trivial code. Or if you don't feel like it.

1. A good method is 2000 lines of code

1. A better method is 200 lines of code, all in one line.

1. Sprinkle the word "TODO" as a comment throughout the codebase, but leave it to others to figure out what its for.

1. Instead of deleting code you don't need, comment it out. But make sure you put "TODO" in a comment above it.

1. In fact, be proactive about scary commented out code. Leave `User.destroy_all` as a comment in the user signin process.

1. Give things names that are deeply related to the codebase, but have nothing to do with whats actually going on.

1. A good codebase is like a puzzle; it should be difficult to assemble and be missing pieces

1. There is no such thing as too many relations. a `user` should `has_many :last_names`

1. "Set it and forget it" is more than just the slogan for a late night cooking infomercial, its a design philosophy.

1. Good conditionals should read like

```
if true
  true
elseif true #TODO
  true
else
  true
```

