---
layout: post
title: "Cucumber"
date: 2015-01-01 22:22:22
categories: [books, reviews]
tags: [books, reviews, 2015]
---
The book even if it's targeted towards the Ruby crowd is as useful for the Ruby as well as for Ruby-derived frameworks, such as Lettuce for Python or Cucumber-java for well, you know...

The first ten or so chapters have a really nice progress of testing and at a same time drafting an application that is of course focussed on Cucumber. Lots of tips as well as ways of working and avoiding common pitfalls are presented throughout the chapters.
Also throughout the book you'll find not only useful Cucumber tailored techniques but also things which apply in a continuous integration environment. As the focus is also on the environment, you can find various Ruby frameworks which can be used for you daily tasks regarding the Cucumber framework.

A couple worth mentioning would be ActiveRecord (working with databases), HTTParty/Sinatra (RESTful services), Capybara (AJAX pages), Database cleaner (consistent state of DB)

Below you can find some "might-be-useful" ideas from the book:
- Tags can be used to execute just certain scenarios
- Different formatters for the output, i.e. `-f rerun` to easily try out failing steps
- Tagged hooks in order to run certain scenarios only on tagged steps
- we can store various profiles in the cucumber.yml file so we run different features/suites depending on situation
    - also more output options to display a HTML report
    - @wip tag for the features being actively developed
- when working with existing systems:
    - start somewhere with a simple scenario, implement a bug report as a scenario
- Approaches on handling various situation: databases, async calls, web applications
- Aruba is a framework designed to test CLI apps; @announce tags can be used for extra logging
- Other platforms: JS cucumber-js, Java cucumber-jvm, C# SpecFlow
    - Gherkin language is one component, if one wants to implement a Cucumber-compatible framework
    - Cucumber wire protocol is the other component, and it's well documented so it should be fairly easy to implement drivers
- Ruby specific: bundler - a sort of Ruby package manager with auto-dependency, 

Rating /5

As usual, you can find the book on [Amazon] or [Goodreads], if you're looking for more information or a place to grab a copy.

[Amazon]: http://www.amazon.com/Cucumber-Book-Behaviour-Driven-Development-Programmers/dp/1934356808
[Goodreads]: https://www.goodreads.com/book/show/12409185-the-cucumber-book
