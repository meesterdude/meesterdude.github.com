---
layout: post
title: "Good software is good writing: the value of refactoring"
date: 2014-02-19 09:44
comments: false
categories: feed geek
published: true
external-url:
---
Without an editor, a story will devolve. Without refactoring, software meets a similar fate.
<!--more-->

When you read a published book, you expect it to be free of typos, grammatical errors and run on sentences. This is a professionally produced item after all, and you paid good money for it. The first draft an author makes is surely filled with mistakes and only carries with it the gist or essence of what is trying to be conveyed. An author can improve upon this draft and come to a reasonable final version given enough time and revisions; but this pales in comparison to what a good editor can bring to the table. Not only will they more readily catch basic errors relating to grammar and spelling, but can also help improve the tone, message, imagery and length of the work as well.

Have you ever read a book that was self published by the author? Too many choose to skimp on an editor and wind up with something that could be classified as torture to read.

If an author just keep writing more chapters and never goes back to what was previously wrote, the book becomes an inconsistent, confusing maze riddled with dead ends and plot holes.

Interestingly enough, software suffers from similar problems. Too often software is written one feature after another, just like one chapter after another, and no attention is paid to the overarching design or storyline. When you refactor, you go back to chapters already written and cut out parts you don't need, or rewrite others to be simpler and clearer. This makes the story of the application easier to understand and support, and lets you add new chapters (or rather, features) later on that you didn't know would be written or necessary. To refactor & edit is to prepare for the unknown future.

But both editing and software refactoring are preferred alternatives to what inevitably comes when a book or application is not groomed: the infamous rewrite. In some instances this is justified and healthy, but more often than not it is a last resort to something that has become unmaintainable and difficult to understand.

One important distinction: Books are often finished once published. Books relating to industry or education are more typically ever-evolving works, but a vast majority are not revisited once they're out the door. This is actually the opposite with software applications: Most are ever-evolving works, with few being truly static or "finished". Even if no features are being added, there will typically be security patches needed, upgrades required to supporting frameworks, or adjustments to handle changes for systems and services that the application relies on.

Editors are not free and nor is refactoring. It takes time and effort to do, and sometimes the changes have questionable value when the alternative is more chapters or features. It is possible to reach a point for an editor to have nothing left to change in a book, and it is possible for an application codebase to reach a point where there is nothing left to refactor. This must all be balanced against budget and time constraints, as well as philosophy of whats important to the book or application; both now and in the future.