---
title: "Read by Refactoring - Part 1: Making Legacy Code more Readable"
date: 2016-08-21 18:55:00
category: post
tags: [code-review, legacy-code]
---

Have you ever spent hours looking at legacy code - making notes on paper, adding comments to the codebase, and cursing poorly named methods and variables - just to have all of that hard work disappear when you switched contexts or took a break?

Last week I went to a workshop at Expedia called ***Read by Refactoring*** where we learned a methodology for reading legacy code and making it better in the process.

[Click here for Read By Refactoring Part 2.][rbr2]{:target="_blank"}

## What is the goal?

The read by refactoring process is designed to capture the work a developer does to comprehend legacy code, by making ***incremental permanent improvements*** to the readability of the code itself. A code reader accomplishes this through liberal use of their IDE's refactor methods, such as rename, extract, and inline, to slowly but surely unravel the core meaning behind a pile of [spaghetti code][spaghetti-code]{:target="_blank"}.

These readability enhancements help you to understand how a chunk of legacy code works more quickly, and allows you to make more targeted changes with fewer unintended side effects. Future team members benefit from your work as well, as they would have surely gone through similar exercises to understand the same chunk of code that you worked to comprehend today.

Frequent commits are a must while making changes, making it easier to determine root cause of an unintended side effect or bug during a set of refactors. Code should never be in a state that does not compile, and manual changes that do not involve your IDE's refactor tools should be made with caution (and marked as RISKY in commit messages).

## Core 6 Refactorings

These are the core 6 different changes that will be made while refactoring your legacy code. To emphasize again: these are all to be made using the IDE's refactor tools, and should not be made manually (unless committed separately and marked as RISKY). The refactor tools make changes that are guaranteed to be safe, while if you go rogue and start making manual changes then anything can happen.

* Rename
* Inline an extracted entity
* Extract a Method
* Extract a Local Variable
* Extract a Field
* Extract a Parameter

These above can be mapped to CRUD operations, all operating on **names and identifiers** in your codebase.

* Create - All of the extract bullets mentioned above.
* Read - Readability is increased by going through this process!
* Update - Rename
* Delete - Inline

The process of creating names for your code has enough content for another blog post...

## Final thoughts

"But what if I'm not using an IDE?"

Well you should be! Not using an IDE with refactor tools like the ones discussed above is a waste of time. IDEs take a lot of the tedious (and uninteresting) thought out of programming, and allow you to focus back on the problems that really matter. They allow you to code faster, smarter, and with fewer dumb mistakes. When I first started at Expedia, I was used to using Sublime Text for all my code editing needs. Using IntelliJ seemed like a ton of unnecessary overhead when all I REALLY needed to get coding was a lightweight text editor... boy was I wrong.

"But what if I'm writing in a dynamically typed / non-compiled language?"

This is a good question. Part of the read by refactoring process is to ensure your code compiles between changes, which isn't really a thing if you're writing in a dynamically typed / non-compiled language like JavaScript or Python. We worked in Java during the workshop, but I'd imagine that the **update** (renaming) operations would be safe to make in the languages I mentioned above as well.

[spaghetti-code]: https://en.wikipedia.org/wiki/Spaghetti_code
[rbr2]: /posts/2017-02-26-read-by-refactoring-pt-2/
