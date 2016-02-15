---
layout: post
title:  'The "Value Add" of Testing'
date:   2016-02-10 19:06:00
category: til
tags: [testing]
---

TIL what the true value add of testing is...__there is none!__

Let me explain - I happened upon [this][this]{:target="_blank"} article from Google's testing blog today while I was looking for clarity on when to write end-to-end (E2E) tests vs. when to just unit test. The post has very strong opinions on the purpose and useful-ness of E2E tests, and the underlying sentiment resonated with me.

"Value" in this instance is defined as direct customer value: something that solves a customer's problem. Testing does not solve customer problems, testing solves __developer__ problems, by letting developers know when parts of the application are not working as expected.

A good test is, therefore, one that __helps a developer pinpoint exactly where in the codebase the unexpected behavior is occurring.__ This allows them to fix the underlying problem faster (the value add step), by zeroing in on the unit of code that caused the test to fail.

A robust unit test suite compliments a goal of faster bug fixes: unit tests are written at the highest level of granularity possible in the codebase, and when they fail a developer knows precisely what failed. E2E tests by definition do not provide this resolution, and when they fail (sometimes randomly), a developer needs to do additional digging to determine root-cause of the failure.

E2E tests have their place, but should be used sparingly in favor of more complete testing at the unit level. The goal should be for a testing [pyramid[1]][pyramid]{:target="_blank"}, not an [ice-cream cone[2]][ice-cream]{:target="_blank"}!

Related Links:

- [1] http://googletesting.blogspot.com/2015/04/just-say-no-to-more-end-to-end-tests.html
- [2] https://watirmelon.files.wordpress.com/2012/01/softwaretestingicecreamconeantipattern.png


[this]: http://googletesting.blogspot.com/2015/04/just-say-no-to-more-end-to-end-tests.html
[pyramid]: http://2.bp.blogspot.com/-YTzv_O4TnkA/VTgexlumP1I/AAAAAAAAAJ8/57-rnwyvP6g/s1600/image02.png
[ice-cream]: https://watirmelon.files.wordpress.com/2012/01/softwaretestingicecreamconeantipattern.png