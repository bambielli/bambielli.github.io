---
layout: post
title:  "Chrome Profiler: Self-Time vs. Total-Time"
date:   2016-02-24 21:40:00
category: til
tags: [chrome, performance, debugging]
---

TIL the difference between self-time and total-time in the Chrome profiler.

When profiling a web-page for performance purposes, I first take a look at the aggregate view to find where the browser is spending most of its time.By clicking on the call-tree tab in the aggregate view, you are able to dive down in to the call stack to get a better sense of where that time is being spent.

Time is broken out in to two columns in the call-tree view: self-time and total-time.

__Self-Time__ represents the amount of time that was spent in the function at the current level of the call tree. This indicates time to execute code *in the function itself* (inline statements), not in any of the functions it might call. A large self-time means you've pinpointed where the browser is spending a lot of its execution time.

__Total-Time__ is self-time + the amount of time it took to execute code in functions that the current level calls. You can dig down deeper in the call tree in to functions called by the current level, following branches with the highest total-time, until you find a function that has a high self-time. Total-time gives you a high level indicator that something in that portion of the call tree is taking up a lot of the browser's execution time.

__So to summarize:__ *start call-tree analysis by digging down in to branches with high total-time, but be on the lookout for levels of the call-tree with high self-time.*

P.S. a high self-time *doesn't necessarily point to a poorly performing function...* it could just indicate that a certain function is being called a LOT, so the browser is spending the majority of its time resolving calls to it. If this is the case, it is still an indicator of potential performance issues in your code, it just might require some more digging to determine the root cause of the inefficiency.
