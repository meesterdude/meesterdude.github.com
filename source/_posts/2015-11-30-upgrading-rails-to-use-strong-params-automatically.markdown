---
layout: post
title: "Upgrading rails to use strong_params automatically"
date: 2015-11-30 08:15
comments: false
categories: feed
published: true
external-url: 
---

If you need to upgrade a rails app to rely on strong_params, getting all the attributes sorted out can be tedious and painful. But here I'll share with you a method that you can use to have most of the work done automatically. All you have to do is exercise your site, or have your test suite do it.


<!--more-->

## Installation

Put the code (below) in `ApplicationController` and set a before_filter like `before_filter :install_strong_params, only: [:create, :update]`. 

This code, when temporarily used within your project, will rewrite your controllers to use strong_params. It is not intelligent, and you may find you need to tweak the code or its output for it to work for your codebase; but it should carry you most of the way at least. 

If you need to debug, uncomment the line that adds a params comment to the bottom of the controller.

Once your app has been upgraded, you should remove the code. 

## Assumptions

 - a blank line preceeding the last line the controller, which should be an 'end' (typical for most apps)
 - existing create/update code is written using the symbol (params[:user] vs params['user'])
 - the first hash value found among the params is the one to use for strong_params (typical for most rails apps)
 - the params being submitted represent everything that should be allowed (not always the case with specs)

## The Code

```
 def install_strong_params
    path = self.class.instance_methods(false).map { |m| self.class.instance_method(m).source_location.first}.uniq.first
    lines = File.open(path).read.lines
    name, hash = params.detect{|k,v| v.is_a?(Hash) }
    return true if hash.nil?
    return true if lines.detect{|l| l.include?("#{name}_params")} # abort if already replaced
    has_private = lines.detect{|l| l.include?('  private')}
    # insert method defition at second to last line
    lines[-2] = %Q^#{"  private \n\n" if !has_private}  def #{name}_params
    params.require(:#{name}).permit(#{hash.keys.collect(&:to_sym).to_s[1..-2]})
  end\n\n^
    # lines[-1] = "end\n\n# #{params}" # uncomment to write params as comment at bottom of controller
    lines.each_with_index do |l,i|
      lines[i] = l.gsub("params[:#{name}]", "#{name}_params") if l.include?("params[:#{name}]")
    end
    File.open(path, 'w+') { |file| file.write(lines.join) }
  end
```
