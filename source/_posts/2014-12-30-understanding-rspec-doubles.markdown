---
layout: post
title: "understanding rspec test doubles"
date: 2014-12-30 07:45
comments: false
categories: feed geek
published: true
external-url:
---
For a while, I have not understood what rspec test doubles do, or why you'd use them. I recently came to understand their usage and function, and thought I'd share.
<!--more-->

If you take a look at [the documentation](https://relishapp.com/rspec/rspec-mocks/docs) you'll find test doubles described as:

> A test double is an object that stands in for another object in your system during a code
example.

I don't know about you, but thats not very descriptive. So, lets take a look at an example using some pseudo rails code

```ruby
class User
  belongs_to :account

  attr_accessor :is_verified

  def can_login?
    self.is_verified && self.account.enabled?
  end

end

class Account
  has_many :users
  has_one :subscription

  def enabled?
    self.subscription.active
  end
end

class Subscription
  has_one :account
  attr_accessor :active
end

```

In the above example, we have defined a `User`, a `Subscription`, and an `Account` class. We want to test `User`, so at first glance you might instantiate your objects like this in test

```
subscription = Subscription.new(active: true)
account = Account.new(subscription: subscription)
user = User.new(account: account, is_verified: true)
```

This will work, but its a bit much. We are just testing `User` and we'd like to test it in isolation. Looking at our example, we see that for a `User` to know if it `can_login?`, it must ask its `Account` which goes on to ask the `Subscription`. We could stub the `Account#enabled?` method and remove the need for subscription like so:

```
account = Account.new
account.stub(:enabled?) { true }
user = User.new(account: account, is_verified: true)
```

This is better, as we no longer need to create a subscription; but we're still instantiating an Account class and everything that gets associated with it. Do we really need a real Account object, or can we use something more lightweight? this is where test doubles come in. They're a bit like stunt doubles, standing in for the real actors when the real actors aren't actually needed.

A further refactoring would look like this:

```
fake_account = double("Account", :enabled? => true)
user = User.new(account: fake_account, is_verified: true)
```

Now we only have one real object to worry about! calls to `user.can_login?` will be delegated to the `fake_account`, which is set to return true for `is_verified`.

Its important to note a few things:

1. If you're concerned with the method your stubbing actually existing, use [verifying doubles](https://relishapp.com/rspec/rspec-mocks/v/3-1/docs/verifying-doubles)(new to rspec 3) to ensure that the method you stub actually exists on the real version of the object. You'll get an exception if you try to stub something that doesn't exist.

1. There are a few types of doubles, covered in the above documentation. This allows you to double not only instances but classes as well, along with a few other testing tricks you might need to resort to such as working with singletons.

1. Do not stub or double what you're testing! Use it when you want to isolate something like a method and test its various conditions, without having to actually manipulate the far reaching objects and logic it relies on to arrive at the same conclusion. Assuming the things you stub or double also have unit tests associated with them, this should not be a problem.

1. You still should have some test coverage that exercises the actual operation of all the parts together. But often you can do this in an integration spec and verify that a lot of things are working properly with a few lines of code. Unit tests are a thousand little strokes of the testing paint brush, integration tests are 100 broad strokes.

Anyway, hopefully this helps.

Special thanks to Kevin Skoglund (@kskoglund) for helping me make heads of doubles. He's got a new lynda.com tutorial coming out on rspec 3 which you'll be able to find [here](http://www.lynda.com/Kevin-Skoglund/104-1.html) once released.
