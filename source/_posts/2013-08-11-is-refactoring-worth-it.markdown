---
layout: post
title: "Is refactoring worth it?"
date: 2013-08-11 14:09
comments: false
categories: feed geek
published: true
external-url: 
---
Here, we discuss if refactoring is worth the time, money, and effort; and where it's not.
<!--more-->
## What is refactoring?
When you want to change how something happens, but not what. In programming, that means changing the structure of the code without altering it's behavior. 

## Why would I want that? I'd rather have features.
The benefits of refactoring are numerous, but it is often unclear to the people who have to pay for it what good it serves. In my travels, I've identified three reasons you would want to refactor a codebase. 

### Maintainability
Code that is easy to work with and easy to understand will result in less bugs, fewer security holes, and greater uptime. It means the people who work with it will be more comfortable approaching the codebase, and anyone new will require less training and support. A maintained codebase is an organized one, with everything in it's proper place, neatly labeled and categorized. 

Try finding a book in the library, and it will take you only a few minutes. But if it was just shelves and shelves of books, you could be there for days trying to find the book you're looking for. It's easy to throw books on a shelf, and it's easy to write code. But it takes time and effort to organize it into an structure that anyone can work with. 

### Adaptability
When a business starts, they don't usually plan on being around for a few months or years. Most of the time, they expect to be around forever. So they'll invest a great deal of time and money into building up codebases to run their company on. And they expect that these codebases will also last forever. 

A well structured project is adaptable to change, both in the short term and long term. It means a new feature or bug fix is as straightforward to implement as feasible, versus the alternative of "it wasn't designed for that". It means when you go from 500 customers to 500 million customers, your project will be able to change to support the additional work. 

It doesn't happen overnight, or by magic. It takes a lot of effort to move a codebase from brand new to that of amazon's, and not every project needs to be scaled that large anyway. But if it's going to last, it needs to be able to adapt. 

### Speed
A codebase can be optimized to operate faster through a variety of techniques, and these kinds of optimizations can only come from refactoring, as typically the initial implementation is simple and unoptimized, but works, or at least did when the developer wrote it. 

But in addition to that, there are other speed gains to be had. Bugs will be quicker to diagnose and resolve, features will roll out sooner, the architecture will support faster scaling, and new developers will be up to speed in less time. 

## When is refactoring not the right call?
Not all code is equal. Typically the kind of code that gets a free pass on refactoring are support scripts, the kind that glue logic and systems together. They are often small enough, and simple enough, to have little or no gain to be had by changing the implementation. But even these can be the subject of refactor if it's deemed appropriate. 

If you rely on it, if others need to work with it, or you need to keep adjusting it, it's probably worth refactoring. 

## What happens if I don't refactor?
You could do nothing, and get away with it for a while. But for a codebase to get running, it typically relies on other systems and libraries to work. Updates are made, features are deprecated, Bugs are fixed, security holes patched, and so on. So while yes, if you are able to lock everything down, there's no need to change anything. But all it takes is one change, one security patch, or one hardware upgrade, and you just collapsed a house of cards. 

For most projects, time wears on them as the world moves on and the codebase does not. Given enough time, and enough neglect, a codebase can quickly become unmaintainable and require significant refactor or an entire rewrite to get working again, or to make an otherwise simple change. 

## How much time and effort should be spent on refactoring?
This is dependent on the stage of the project and the priorities at play, so "it depends". Typically once a project has solidified most of it's business rules and met it's core deadlines, and is faced with a "ok, what's next?" question, is a good time to start focusing more on refactoring. The rule of thumb is somewhere between 60/40 and 80/20, between refactoring and new features. 

But in the end it's a juggle of short-term priorities and long-term ones, and those are obviously subject to change any given day. But if you focus on the short term and never on the long term, there won't be a long term.

Maybe thats a decision you have to make, or a risk you have to take. At least now you can do so in an informed manner. 

## TL;DR
To answer the question I posed in the title: Typically, yes. 