---
layout: post
title: "pjax_rails and layout rendering"
date: 2014-08-25 01:41
comments: false
categories: feed
published:
external-url:
---
An issue I ran into when using the `pjax_rails` gem, and useful advice if you need additional content delivered besides the `yield` result.
<!--more-->
I ran into this issue and my googlings returned little information, so I PR'd additions to the `pjax_rails` gem readme, and thought I'd
make a post while I'm at it for anyone who runs into this issue in the future.

When you add `pjax_rails` to your project, it ensures every pjax request does not render your layouts, and only the relevant view.
But you may run into the need to include additional content, such as a contextual menu, outside of the particular views.

You can specify a particular layout to render for pjax requests, by overriding the `pjax_layout` method in your controllers.

```ruby
class ApplicationController < ActionController::Base
  def pjax_layout
    'pjax'
  end
end
```

And also creating the relevant file `app/views/layouts/pjax.html.erb`.

You'll likely want `application.html.erb` and `pjax.html.erb`
to render the same content, so I recommend moving that content (the `data-pjax-container` element and its children) into a
partial, and then rendering that out in both `application.html.erb` and `pjax.html.erb`.

As a tip, I add a `h2` title to `pjax.html.erb` when I want to debug what's pjax and what's not.