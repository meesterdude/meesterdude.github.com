---
layout: post
title: "Understanding the monty hall problem"
date: 2015-02-19 23:36
comments: false
categories: feed
published: true
external-url: 
---
I explore the classic monty hall problem and use a little bit of ruby to help understand it.

<!--more-->
{% blockquote wikipedia http://en.wikipedia.org/wiki/Monty_Hall_problem %}
Suppose you're on a game show, and you're given the choice of three doors: Behind one door is a car; behind the others, goats. You pick a door, say No. 1, and the host, who knows what's behind the doors, opens another door, say No. 3, which has a goat. He then says to you, "Do you want to pick door No. 2?" Is it to your advantage to switch your choice?
{% endblockquote %}

The answer is yes, but why? Why does switching have a higher rate of success? 

Given a choice of 3 doors, its fair to say that your odds of being right are 1/3. When the host reveals a door he knows to be a dud, this does not change your original choice success rate - you still have a 1 out of 3 chance. But there is now a 2/3 chance that the other door is the one with the prize. In essence, you're betting that your original guess was wrong (it only has a 1/3 chance of being right) and that this other door, with all others doors eliminated and yours not very likely, to be the better choice.

This is much more apparent with something like 100 doors, because your original chance was 1 in 100 of picking correctly, but the chance of it being the remaining door out of 99 is much more likely. 

Thankfully, this isnt that hard of a problem to create in code. Feel free to paste this into a ruby console and experiment! 

```
def monty_hall(door_count,pick, switch=false, runs=100)
  results = []
  runs.times{results << perform(door_count,pick,switch)}
  summary = results.group_by{|v| v}
  {won: summary['won'].count, lost: summary['lost'].count}
end

def perform(door_count, pick, switch=false)
  door_count += -1 # make room for the prize
  pick = pick.to_i - 1
  doors = door_count.times.collect{'1'} << '2'
  doors.shuffle!
  prize = doors.index('2')
  same = prize == pick # did they manage to pick the prize?
  dud = doors.index('1') # a known dud to use if same
  doors.each_with_index do |door,i|
    if same
      next if [pick,dud].include?(i) 
    else
      next if [prize,pick].include?(i)
    end
    doors[i] = nil # if it wasn't one of the two skipped, clear its value
  end
  # we now have an array that looks something like [nil,2,1]
  if switch
    doors.each_with_index{|d,i| new_pick = i if (d.to_i > 0 && i != pick)}
    new_pick == prize ? "won" : 'lost'
  else
    same ? "won" : "lost"
  end
end

#
# Examples
#
#> monty_hall(3,3,true)
#=> {:won=>62, :lost=>38}
#
#> monty_hall(3,3,false)
#=> {:won=>33, :lost=>67}
#
#> monty_hall(100,5,true)
#=> {:won=>99, :lost=>1}
#
#> monty_hall(100,5,false)
#=> {:won=>4, :lost=>96}

```
