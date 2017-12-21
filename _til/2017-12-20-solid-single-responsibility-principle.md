---
title:  "SOLID: Single Responsibility Principle"
date:   2017-12-20 18:51:00
category: til
tags: ["solid", "single", "responsibility", "principle", "single responsibility principle", "single-responsibility-principle", "architecture", "design", "software"]
---

TIL about the `single responsibility principle`, and how it can lead to decoupled classes that make changes less risky.

The `single responsibility principle` is the `S` in **S**OLID, and it's all about separation of concerns in your code.

The principle states that **a class should only have one reason to change.** If an entire system is designed in this manner, then the impact of changes to the system are isolated to a smaller number of classes. This reduces the potential for bugs as your code changes over time.

Another way to state the `single responsibility principle` is: **Gather together the things that change for similar reasons in common classes. Separate the things that change for different reasons.**

This phrasing is still all about minimizing the impact of change in your codebase. If methods that change for the same reasons are gathered together in a common class, then you minimize the number of points in your codebase you need to touch when changes occur.

A classic example of respecting the `single responsibility principle` with your class design, is when you **separate the persistence of your objects from any business logic pertaining to those models.** In this instance, you are gathering together the things that change together and separating the things that don't.

The business logic pertaining to a set of models might change often as requirements change. This shouldn't cause the code that persists your models to change as well.

Conversely, if you choose a new persistence method for your models (switching from MySql to Postgres, for example) it shouldn't necessitate a change in the code where your business logic resides.

Every time you touch a file, **there exists potential for bugs to inadvertently sneak in to your code**. By gathering together the things that change for similar reasons, you ensure that the amount of code exposed to a change is minimized.

This is the essence of the `single responsibility principle`.
