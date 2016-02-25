---
layout: post
title:  "Using the Chrome Profiler"
date:   2016-02-23 21:14:00
category: til
tags: [chrome, performance, debugging]
---

TIL how to use the Chrome "Timeline" Profiler to help uncover poorly performing front-end code.

Recently, I've been working on the front end code for a large scale re-architecture of one of our grid components. I hooked the grid up to the re-architected API, and was disappointed with the performance of the grid overall (scrolling had a lot of jank). I decided to profile the javascript while I scrolled to  find the source of the poorly performing code.

Out of a 6 second profiling window, __around 90% of the time was spent in scripting.__ By diving in to the call-tree of the scripting category, I uncovered a ton of angular-animate events being triggered on each scroll event.

Each cell of our grid contains a host of custom directives, links, and tooltips, that apparently are re-drawn on each small scroll. These trigger angular-animate events (removing and adding elements to the DOM) which slows the final rendering of new rows down considerably.

The most confusing part of digging in to the call tree was trying to figure out what was calling angular-animate in the first place... while diving in to a particular call stack, I saw anonymous functions calling anonymous functions on end, inevitably ending in a jquery library call. This wasn't very helpful.

To figure out what was triggering these events, I logged arguments in the animate and jquery library methods being called, which pointed me to the re-drawn elements in each cell.

I wasn't aware that those elements needed to be removed and re-added to the DOM as they moved up the screen, but I guess it kind of makes sense (kind of). We will need to find a way to simplify the directives being created for each cell, or reduce the complexity of each cell in general.

I'm sure I've just scratched the surface of what the profiler is capable of, but it was definitely helpful in highlighting where the performance bottlenecks were!
