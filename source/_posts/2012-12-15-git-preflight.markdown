---
layout: post
title: "Git Preflight"
date: 2012-12-15 23:21
comments: false
external-url: 
categories: feed geek
published: 
external-url: 
---
I wrote this script because I kept commiting errors to github and wanted to avoid the really stupid mistakes; for me thats was commented code, binding.pry entries, console.log entries, and failing specs. I use it every day, and felt like sharing.
<!--more-->
I'm lazy so I aliased this command to 'a' and skipped needing an opening quotation mark. in my ~/.bash_profile I have `alias c="gitpreflight '"`. So for me, a commit is a matter of typing `c [MVC] updates new_resource'`. It is also obviously in my command path, with this entry also in the file `export PATH="$HOME/bin:$PATH"` and the actual script residing in `~/bin/`

This script will do some basic checks to make sure you aren't commiting anything questionable, and will highlight anything in the diff if you do. It's easy to extend if you have other things you want to stop yourself from commiting, such as perhaps a 'FIXME' comment you left for yourself. Lastly, if you haven't run the spes and think theres any chance you might have broken them, it's a good idea to run them.

Lastly, if there's anything else I want to add or do, I don't have to change my workflow. All I have to do is tie it into the script, a bit like callbacks! A few ideas for such things would be:

  * reminder to stretch, meditate, etc.
  * ask for time worked and populate time entry
  * tracking test runtime and logging to datastore for trending


**Feel free to use, improve, and share.**

``` ruby Git Preflight https://gist.github.com/43bb316be1bb5d126d1b View Github Gist
#!/usr/bin/ruby
 
# ©2012 Russell Jennings. Released under MIT license.
 
class String
  def colorize(text, color_code) "\e[#{color_code}m#{text}\e[0m" end
  def red()     colorize(self, 31) end
  def green()   colorize(self, 32) end
  def yellow()  colorize(self, 33) end
  def pink()    colorize(self, 35) end
end
 
puts "#### Git PreFlight ####".pink
puts "#### ©2012 Russell Jennings ####".pink
sleep 1
puts "checking staged files for errors"
sleep 1
gitdiff =%x[git diff --staged]
if gitdiff.empty?
  puts "nothing staged for commit. exiting...".yellow
  exit 0
end
@gitlines = gitdiff.split("\n")
@errors = Array.new
i = 0
for line in @gitlines
  case 
    when line.match(/binding.pry/)
      @gitlines[i] = line.yellow
      @errors.push "found pry entry"
    when line.match(/console.log/)
      @gitlines[i] = line.yellow
      @errors.push "found console.log entry"
    when line.match(/^\+{1}.*#+/)
      @gitlines[i] = line.yellow
      @errors.push "found possible commented out code"
    when line.match(/^\+{1}/)
      @gitlines[i] = line.green
    when line.match(/^-{1}/)
      @gitlines[i] = line.red
  end
  i = i + 1
end
if @errors.size > (0)
  @gitlines.each {|line| puts line}
  puts "### WARNING ###".red
  puts "possibly found the following #{@errors.size} errors:".yellow
  @errors.each {|err| puts err.yellow }
  puts "Continue committing with #{@errors.size} possible errors? (y)/n".pink
  exit 0 if STDIN.gets.chop.downcase =~ /n/
else
  puts "no errors found"
  puts "want to see the commit diff? y/(n)".pink
  @gitlines.each {|line| puts line} if STDIN.gets.chop.downcase =~ /y/
end
 
puts "Should I run the specs before committing? y/(n)".pink
if STDIN.gets.chop.downcase =~ /y/
  puts "Run [A]ll, [C]ontrollers, or [M]odels?".pink
  what2run = STDIN.gets.chop.downcase
  case what2run
    when 'a'
      puts "running all specs..."
      runcommand = %x[bundle exec rspec spec]
      exit_code = $?.exitstatus
    when 'c'
      puts "running controller specs..."
      runcommand = %x[bundle exec rspec spec/controllers]
      exit_code = $?.exitstatus
    when 'm'
      puts "running model specs..."
      runcommand = %x[bundle exec rspec spec/models]
      exit_code = $?.exitstatus
    else
      puts "invalid option supplied"
      exit 1
  end
  case exit_code
    when 1
      puts runcommand
      puts "Specs failed!".red
      puts "Continue with commit? y/(n)".pink
      exit 0 unless STDIN.gets.chop.downcase =~ /y/
    when 0
      puts "all specs passed"
  end
end
 
commit = %x[git commit -m "#{ARGV[0]}"]
puts commit
puts "### Commit complete ###".pink
```
