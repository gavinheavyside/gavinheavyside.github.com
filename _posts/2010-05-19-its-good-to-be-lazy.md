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

At first I was baffled, but that was mostly down to trying to read it
on my iPhone after a couple of glasses of wine.  With a clear head and
a big screen the next day it became obvious how he had replaced my
naive recursive algorithm with a much more idiomatic lazy sequence
version that has better performance. 

He defines a function that calculates the next number in the
sequence, and creates a (infinite) lazy sequence of them using
`iterate`. It takes 50 numbers from this sequence using `rest` and
`take` and checks to see if any of them are palindromic using `some`.
I was initially confused by the `->>` macro, but it is explained 
[here](http://richhickey.github.com/clojure/clojure.core-api.html#clojure.core/-%3E%3E).

It's going to take me a while to think naturally in Clojure idioms,
but I think it will be worth it. Paul Graham argues that
[the truly serious hacker should consider learning Lisp](http://www.paulgraham.com/avg.html),
and I think he is
absolutely right. The advantage of being able to think about solutions
to problems in a different way from the dominant procedural and OO mindset
can be really valuable, and I agree that even if you don't
subsequently use Lisp, having learned it will make you a better
programmer.
