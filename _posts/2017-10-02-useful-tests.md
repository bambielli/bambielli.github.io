---
title:  "Writing Useful Tests for React Applications"
date:   2017-10-02 21:59:00
category: post
tags: [presentation, testing, react, javascript, frontend, jest, enzyme, snapshot]
---

My team at Expedia recently put some thought into "what makes a useful test". Read on to hear our thoughts.

## The Golden Rule

Our team arrived at a golden rule for testing: **write tests that are useful.**

This seems like a pretty simple realization, but it has profoundly impacted how we write tests.

Previously we strived (strove?) to meet quantifiable testing metrics like 80% test coverage or 90% branch coverage... we found these metrics did more harm than good. **We realized we were spending more time writing and maintaining our test suite than writing the application code itself.**

Something needed to change.

## What Makes a Useful Test

We spent time thinking about what made a useful test. The 3 axioms we came up with are as follows:

1. Tests that catch bugs before they reach prod are useful.
2. Tests that point developers to the source of a bug are useful.
3. Tests that are easy to write and maintain are useful.

We are now constantly questioning our test suite as we make changes to application code. If we think a particular test isn't useful anymore, we delete it. This ensures our test suite is always lean.

## Presentation

Find the slides from our presentation below.

[If you are on mobile, click here for a PDF of the presentation.][here]

<embed src="/assets/pdf/useful-tests-for-react-applications.pdf" />

[here]: /assets/pdf/useful-tests-for-react-applications.pdf
