---
layout: post
title: "when a+b+c != c+b+a"
date: 2013-02-26 17:53
comments: false
categories: feed rails geek
published: true
external-url: 
---
AKA Time math is not normal math. A subtle bug I caught recently in a project.
<!--more-->
Background
---------
Normally, when you combine a few numbers together, the order in which they combine is irrelevant. 

`1 + 2 + 3 - 1 => 5`

`3 - 1 + 2 + 1 => 5`

Kicker
-------
We had an acceptance spec in a rails project that kept failing. Here's the line that was giving us trouble (and for reference, Subscription::GRACE_PERIOD is 1.day):

{% codeblock lang:ruby %}
 @subscription.expires_at.to_s(:date_only).should == (Time.now + 1.month + 5.days - Subscription::GRACE_PERIOD).to_s(:date_only)
{% endcodeblock %}

Everything as far as we could see was correct. Earlier in the spec we were logging in the user 5 days from now, and they should have access for a month since then, minus a day. We became sure it was an issue with UTC, so We tried this: 

{% codeblock lang:ruby %}
 @subscription.expires_at.utc.to_s(:date_only).should == (Time.now + 1.month + 5.days - Subscription::GRACE_PERIOD).utc.to_s(:date_only)
{% endcodeblock %}

But that didn't work either. 

{% codeblock lang:ruby %}
    expected: "3/30/13"
    got: "4/2/13" (using ==)
{% endcodeblock %}


Catch
-------
After a few hours of tearing apart the codebase we were testing, it started to become apparent what was going on. 

This is what we were using to determine the date.
{% codeblock lang:ruby %}
(Time.now + 1.month + 5.days - Subscription::GRACE_PERIOD)
{% endcodeblock %}

Let's take that apart and see whats going on.

{% codeblock lang:ruby %}
Time.now 
=> 2013-02-26 03:24:45 -0500

Time.now + 1.month
=>2013-03-26 03:24:08 -0400

Time.now + 1.month + 5.days
=> 2013-03-31 03:25:34 -0400

Time.now + 1.month + 5.days - Subscription::GRACE_PERIOD
=> 2013-03-30 03:25:54 -0400
{% endcodeblock %}

*The observant reader will note the change in UTC offset when we added a month to Time.now; this is because of DST.*

However, it turns out that this is not how the code under test does it, once broken down. It was instead doing this something more like this:

{% codeblock lang:ruby %}
Time.now 
=> 2013-02-26 03:24:45 -0500

Time.now + 5.days
=> 2013-03-03 03:24:24 -0500

Time.now + 5.days - Subscription::GRACE_PERIOD
=> 2013-03-02 03:26:21 -0500

Time.now + 5.days - Subscription::GRACE_PERIOD + 1.month
=> 2013-04-02 03:26:50 -0400
{% endcodeblock %}

which means the code we need to get this spec to pass looks like

{% codeblock lang:ruby %}
 @subscription.expires_at.to_s(:date_only).should == (Time.now + 5.days - Subscription::GRACE_PERIOD + 1.month).to_s(:date_only)
{% endcodeblock %}

Confused?
---------
A simpler version highlights the issue

{% codeblock lang:ruby %}
Time.now + 5.days + 1.month  
{% endcodeblock %} 

Thats the 26th, + 5 days puts us into 03/03, + 1 month gives us 04/03. Lets look at the correct version.

{% codeblock lang:ruby %}
Time.now + 1.month + 5.days 
{% endcodeblock %} 
The 26th again, + 1 month is 3/26, + 5 days is 03/31

When doing math that deals with time, specifically different units of time, it can lead to unexpected results if you are not careful with the ordering. 