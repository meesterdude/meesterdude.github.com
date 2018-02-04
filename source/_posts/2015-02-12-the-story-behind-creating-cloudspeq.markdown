---
layout: post
title: "The story behind creating Cloudspeq"
date: 2015-02-12 14:28
comments: false
categories: feed geek rails gem
published: true
external-url: 
---
How I got my 5 minute test suite down to 20 seconds, and made my first gem from my efforts. 

Get it here:[rubygems.org](https://rubygems.org/gems/cloudspeq) and [github.com](https://github.com/meesterdude/cloudspeq)


<!--more-->
I wanted to cover the important takeaways and how I got there, and this is largely geared towards that more than every detail of the proccess. 

I recently switched my development enviroment over to a linux virtual machine - something I intended on blogging about a few months down the line after the honeymoon phase has passed. Everything has been pretty decent so far, except for a slow down in my test runtime. The test suits for the Ruby on Rails apps I work on aren't normally fast on a good day, and were a little more sluggish in my VM. So I wondered if there was a way to speed up my test time locally somehow. I tried a few solutions but none of them really made much of a difference.

I thought about what people have classicly done to speed up test suits. Some people have gone so far as to decouple their code from rails and test in isolation - but thats controversial and nontrivial. I tried paralleism and got a marginal benefit, but not nearly what I was expecting. Eventually, I just wanted to copy my virtual machine and switch between a few of them to execute my tests. An idea I quickly disspelled, but it led me to my first Ah-Ha...


## Ah-Ha #1: Its just a computational problem

The tests we write need to be executed to ensure our code works. Some Ruby on Rails projects can run their entire test suite in a few seconds, but many rails projects that I work on do complicated browser testing which just takes time to run through; sometimes a really long time. 

CGI animators are not unfamiliar with the problem of waiting for computers. They can work on scenes that would take a normal computer hours or days to try render a single frame for. Their solution is to break up the work across many machines and have them all work on a little piece. It's basically the same thing only here, instead of rendering CGI animations, we need to render test results. 

In practice, this works well. But not everyone can afford a server farm to have ready for them to run tests for! I know I can't. There will need to be a way to start things up on-demand...


## Ah-Ha #2: Digital Ocean and its handy API

With Digital Ocean, I can take a snapshot of a machine and turn that into an image I can then later use to create new machines from. The new machine has a new ip-address and hostname, but otherwise everything else is the same.

And with their API, all I need to do is create a few machines with a predefined image, and I have an on-demand server farm. The costs are pretty good - $0.175 for 25/machines/hr; worth it if it can shave minutes off my testing time and I'll be doing a lot of testing. 

Its worth pointing out that machines take a few minutes to spool up - especially if you create a bunch at once. 

So, I create a few machines to execute tests for and break up all the files in the test folder to run across the machines. I notice a good order of impovement in testing time - enough to confirm my suspicions that it it'll work. 

Some machines got stuck with all the slow tests, so I shuffled the file ordering before assigning them to machines - so each got a few different kinds of files to work on, which should even out the overall test run time. 

But the tests still weren't *quite* as fast as I'd like them to be. Some files would have a lot of slow tests in them, and whatever machine got stuck with that file inevitbly slowed down the test run time. 


This lead to...


## Ah-Ha #3: Que on the specs themselves, not the files that hold them

By issuing specific line numbers instead of whole files, slow test files could be torn open and their slow inards spread across several eager machines, resulting in a good deal of improvement in the time it takes to test. 

But the tests *still* felt slow. Even though I broke open these slow spec files, their sluggish guts now polluted the test time of the rest of the machines and prevented them from finishing faster. I needed a way to break them up but isolate them from the rest of the machines. Actually, I wanted to be able to break anything up - specs, files and whole directories. This all lead to...


## Ah-Ha #4: Clusters

With clusters, I could give every slow acceptance test its own server, or assign all the helper tests to one server particularly. With some tuning of what got divided up and how, I was able to achieve a testing time that felt reasonable - It was as fast as my slowest spec took to run. 

After this, it was pretty easy. I would run the rspec command with a json output formatter, and then parse the results from each machine and display them as the overall test results. I went to bed pretty happy with what I had accomplished with clusters.

But there was a problem... I forgot to destroy the test machines I created. It ended up costing me about $2; but it was a lesson that lead to...


## Ah-Ha #5: Self destruction

While one could query the Digital Ocean API for machines with `created_at` that is too old and destroy the results - it means you'd need to have a machine out there doing that for you. You could use your development machine, but if you shut it down and go home for the day those machines will keep running. 

Instead, it would be good if the machine could just clean up after itself, since it's already out there running. But this needs to be flexible - if I have a solid day of testing, I don't want to have machines destroyed that I'm working on. Equally I don't want to set it to be too high and spend money on machines I'm not using. 

So, the metric I settled on was uptime. if I set a lifetime of 4 hours and I'm 3 hours in, I can do a quick reboot and have another 4 hours. I could also probably do some bash trickery, or more simply change the config value in the file. Any of those is better than having to wait for another batch of machines to be created!

With this in place, machines that have been alive for too long can self-destruct, and I can go to bed and sleep soundly. 

With that in place, I had the foundation figured out. The rest of it was squeezing it into a gem and settling on a command line parsing gem to use. I went with `escort`; so far its holding up. 


## The Future

Aside from more providers and overall polish, I think this gem could do more for Ruby on Rails testing overall. We often write code but have no idea about how it will perform in production. Technically the `exec` command in the gem already makes it possible to run a command across all the machines - which could be a command to hammer your staging server with requests for a few seconds; but I think something more intergrated could be achieved too.

Also, I feel like the machines could be driven harder, but I haven't had much success in doing so. Ideally i'd cut the number of machines I need to test in half - but its a minor pain point. 

I don't know how useful this gem will be in the run of things - i know i'll use it when I have to test a lot, and maybe there are folks out there who have to suffer through a slow test suite and this will really help them - I released it as a gem with hopes that I am able to contribute something useful back to the community. 