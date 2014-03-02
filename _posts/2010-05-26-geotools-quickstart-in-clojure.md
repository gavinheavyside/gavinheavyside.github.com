---
layout: post
title: "GeoTools Quickstart example in Clojure"
description: ""
category: 
tags: [clojure, programming, gis]
---
{% include JB/setup %}

As part of my [Clojure](http://clojure.org) experiments I'm taking a
look at [GeoTools](http://geotools.org) and the
[Java Topologya Suite](http://tsusiatsoftware.net/jts/main.html) for working with
geospatial data.

Clojure has been described as "A better Java than Java".  I'm not a
Java programmer, but having access to Java libraries in Clojure is
very useful, and Clojure has made the interop remarkably painless so
far.

The [quickstart example](http://www.geotools.org/quickstart.html#quickstart) on the
GeoTools documentation pages starts with a
[horrid Maven incantation](http://www.geotools.org/quickstart.html#depending-on-geotools) 
which I converted to a
[leiningen](http://github.com/technomancy/leiningen) project file. The
[source itself](http://svn.osgeo.org/geotools/tags/2.6.4/demo/example/src/main/java/org/geotools/demo/Quickstart.java) 
once converted to Clojure looks like this: 
{% gist 415029 %}

And the leiningen project.clj:
{% gist 415031 %}

Running either of the functions pops up a dialog box for you to choose
a shapefile, which is then loaded and displayed.

The clojure version is shorter than the Java version, can be tinkered
with at the REPL, and doesn't use any Maven.
