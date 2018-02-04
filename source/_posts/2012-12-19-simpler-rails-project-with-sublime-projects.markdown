---
layout: post
title: "Simpler Rails with Sublime Projects"
date: 2012-12-19 14:13
comments: false
categories: feed geek ST2
published: 
external-url: 
---
Hide the files you don't normally work on with the Sublime Text 2 Project feature. Includes example for Ruby on Rails. 
<!--more-->

Background
-----------
I work on Ruby on Rails sites in ST2 in my day to day work. For bigger projects, its easy to get lost in the files and folders. There are generally 3 levels of view that we want to achieve:

1. files you work on often
2. files you work on rarely
3. files that you never touch

So with Projects in ST2, we can change what level of view we have, by selectively filtering out files and folders. Most of the time, you'll want to be at #1, sometimes you'll need to drop down to #2, and once in a blue moon drop down to #3.

Business
------

The below file is an example of #1 for a Ruby on Rails project. I'll leave #2 up to you to sort out, use this as a template. #3 for me would be the default project view, such as opening the directory directly; but you may want to craft something for that perspective as well. 

Of note: this moves the 'migrate' folder out of 'db' and makes it a top level item, outside of the project folder. I feel it makes it easier to work on that way, but that's purely my preference. 

Put this file in the root of your project with a name like "simple.sublime-project" then open it from the 'Project' menu.

* Learn more about Projects in ST2 [Documentationn](http://www.sublimetext.com/docs/2/projects.html) 
* Become a ST2 power user [Video Tutorials](https://tutsplus.com/course/improve-workflow-in-sublime-text-2/)

``` javascript Simpler Rails https://gist.github.com/4339635 View Github Gist
{
    "folders":
    [
 
    {
        "path": "/",
            "folder_exclude_patterns": [
                "app/mailers",
                "app/helpers",
                "app/assets/images",
                "coverage",
                "doc",
                "config/locales",
                "config/initializers",
                "config/environments",
                "config/deploy",
                "db",
                "features",
                "lib",
                "log",
                "public",
                "script",
                "test",
                "tmp",
                "vendor",
                "spec/mailers",
                "spec/helpers"
            ],
            "file_exclude_patterns": [
                "README.rdoc",
                "Rakefile",
                "Gemfile.lock",
                "config.ru",
                "Capfile",
                ".rspec",
                "db/seeds.rb",
                "db/schema.rb",
                "config/application.rb",
                "config/boot.rb",
                "config/cucumber.yml",
                "config/database.yml",
                "config/environment.rb"
            ]
        },
        {"path": "db/migrate"}
        
    ]
}
```