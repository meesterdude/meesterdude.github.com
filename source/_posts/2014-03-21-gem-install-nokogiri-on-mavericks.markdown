---
layout: post
title: "gem install nokogiri on mavericks"
date: 2014-03-21 16:56
comments: false
categories: feed
published: true
external-url:
---
Documenting my experience installing nokogiri 1.6.1 with ruby 2.1.1 on OS X mavericks
<!--more-->

Trying to spin up a new rails 4 app, `bundle install` failed on the nokogiri install, and asked that I try to install it manually first. When I'd run `gem install nokogiri -v '1.6.1'` it would hang on install indefinitely.

I tried several different things like installing a different version of grep as noted in [this stack overflow](http://stackoverflow.com/questions/20890808/install-nokogiri-1-6-1-under-ruby-2-0-0p353-rvm-based-installation-fails-osx) but that did not resolve.

It did however, compile fine when I switched to ruby 1.9.3.

running `gem install nokogiri -v '1.6.1' --verbose` I saw it was failing here

```
Successfully installed nokogiri-1.6.1
Parsing documentation for nokogiri-1.6.1
Parsing sources...
unable to convert "\xE4" from ASCII-8BIT to UTF-8 for ext/nokogiri/tmp/x86_64-apple-darwin13.1.0/ports/libxml2/2.8.0/libxml2-2.8.0/doc/examples/testWriter.c, skipping
unable to convert "\xF8" from ASCII-8BIT to UTF-8 for ext/nokogiri/tmp/x86_64-apple-darwin13.1.0/ports/libxml2/2.8.0/libxml2-2.8.0/entities.c, skipping
```

So, it turns out it was installing fine, but was hanging on building the documentation, which I never use anyway. So, rerunning the command with `gem install nokogiri -v '1.6.1' --verbose --no-ri --no-rdoc` worked like a charm. Not what I would call a "fix", but at least I can sidestep the problem.

Since I never use documentation anyway, i added the following to my `~/.gemrc` file which should also speed up my gem compile time:

```
gem: --no-rdoc --no-ri
```