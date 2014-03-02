---
layout: post
title: "Running a Jekyll blog on Heroku"
description: ""
category:
tags: [jekyll, heroku, github]
---
{% include JB/setup %}

This blog used to be hosted on the
[Jekyll](http://github.com/mojombo/jekyll)-powered
[Github Pages](http://pages.github.com), and I was using the Custom Domains 
feature to have it appear under with the
[DataMangling](http://datamangling.com) domain name. I've switched to  
hosting it with [Heroku](http://heroku.com) so that I can save myself
$84 per year.

Github only let you use Custom Domains if you've got a paid account. I
had a micro (7$/month) plan, initially because I wanted to keep a
couple of my repositories private, but I don't need to do that
anymore.  I'm on a bit of an economy drive at the moment, so the
monthly payment to Github has been a casualty of my belt-tightening.

The only tangible consequence is I can no longer alias my own domain
name to the blog. This is a bit annoying after I'd gone to the trouble
of finding a dotcom domain that had the word data in it.  The solution
is to host the blog on [Heroku](http://heroku.com), as the free tier
of service is more than sufficient, and I can use my own domain name.

The only question was how to get Heroku to serve the Jekyll blog
properly?

Enter [Rack-Jekyll](http://github.com/bry4n/rack-jekyll).  By
depending on the rack-jekyll gem and adding a config.ru for Rack
awareness the transition was smooth and painless.  The only steps
involved were:

* Add `rack-jekyll` to the .gemfile
* Add a config.ru as described in the [rack-jekyll page](http://github.com/bry4n/rack-jekyll)
* Make sure the `_site` directory is checked into git
* Create a new heroku app and add my custom domain name
* Update DNS to point to Heroku
* Push to Heroku

