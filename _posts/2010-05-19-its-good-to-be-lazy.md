---
layout: post
title: "It's Good to be Lazy"
description: ""
category:
tags: [clojure, programming, craftsmanship]
---
{% include JB/setup %}

On 14th May 2010 the inaugural meeting of the
[Software Craftsmanship UK user group](http://softwarecraftsmanship.co.uk) was held at the
offices of [Eden Development](http://edendevelopment.co.uk) in
Winchester.

The meeting kicked off with introductions, and then
[Enrique](http://twitter.com/ecomba) called Doug Bradbury and
Micah Martin of [8th Light](http://8thlight.com) on Skype, and they
talked to us about the history of the Software Craftsmanship
movement.

After lightning talks, we moved on to a randori-style
[coding dojo](http://codingdojo.org/cgi-bin/wiki.pl?RandoriKata) in which the
task was to write an algorithm to determine how many
[Lychrel numbers](http://en.wikipedia.org/wiki/Lychrel_number) there are in the
starting range 1-10000.

We used Ruby, which was known by the majority (but not all) of
the attendees, and after a few false moves a recursive  algorithm took
shape and an answer was found.  The program could have used a bit of
refactoring, but it satisfied the task.

I'm currently very enthusiastic about [Clojure](http://clojure.org),
partly because I've been meaning to learn a functional language for
ages and this looks like a good one, and partly because it runs on the
JVM and has some [interesting](http://github.com/nathanmarz/cascalog)
[libraries](http://github.com/liebke/incanter) that I want to use with
Hadoop for processing and analysing big data sets at
[Journey Dynamics](http://www.journeydynamics.com).

The next day I knocked up a
[Clojure solution](http://github.com/gavinheavyside/lychrels/blob/16e0d357a7b0a32115dfecafcd3b545af07766d2/src/lychrels/core.clj)
to the problem we'd seen in the dojo.  It was concise, tested, and I
was fairly pleased with it. I tweeted about it, and was about to
forget about it until [@t_crayford](http://twitter.com/t_crayford)
said that he could do better.  Before long he'd posted an improved
version of the main function:
{% gist 402335 %}

At first I was baffled, but that was mostly down to trying to read it
on my iPhone after a couple of glasses of wine.  With a clear head and
a big screen the next day it became obvious how he had replaced my
naive recursive algorithm with a much more idiomatic lazy sequence
version that has better performance. 

