---
title:  "SOLID: Open/Closed Principle"
date:   2017-12-21 16:13:00
category: til
tags: ["solid", "open", "closed", "principle", "open/closed principle", "open closed principle", "architecture", "design", "software"]
---

TIL about the `open/closed principle`, and how it can be used to ensure your code is adaptable to new requirements while remaining resilient to changes.

The `open/closed principle` is the `O` in S**O**LID, and like the [single responsibility principle][srp]{:target="_blank"} it is, at its core, all about reducing the impact of changes to your system.

The principle states that **a class should be open for extensions, yet closed for modification.**

To state it more plainly: when requirements change, instead of needing to modify code that already exists to conform to those requirements, **one should be able to adapt or extend previously written modules to write new code to support new requirements.**

This way, code that was previously working to satisfy old requirements _remains working._

A traditional example of the open/closed principle in action is the [decorator pattern.][decorator]{:target="_blank"}

A decorator is used to modify the behavior of a wrapped class by delegating to the implementation of wrapped methods and "decorating" the result with new behavior. **New code is isolated to the decorator, and the implementation contained within the wrapped class remains un-modified.**

In this way, the wrapped class is open for extension (i.e. it implements a common decorator interface) but is closed for modification. Changes are isolated to decorators, and any number of new decorators can be added as needed.

In this sense, the open/closed principle is just another way of limiting the impact of change within your code.

[srp]: /til/2017-12-20-solid-single-responsibility-principle/
[decorator]: /posts/2017-04-02-decorator/
