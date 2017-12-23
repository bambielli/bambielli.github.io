---
title:  "SOLID: Liskov Substitution Principle"
date:   2017-12-21 16:13:00
category: til
tags: ["liskov", "substitution", "principle", "liskov substitution principle", "liskov-substitution-principle", "architecture", "design", "software"]
---

TIL about the `liskov substitution principle`, and how it ensures the "correctness" of a program.

The `liskov substitution principle` is the `L` in SO**L**ID, and was introduced by computer scientist Barbara Liskov in 1987.

It states that **If class S is a subtype of class T, then instances of T may be replaced by instances of S without altering any of the desirable behaviors of T itself**.

Phew that is rough to parse through... let's go through a classic example.

A relationship that can be modeled to violate the liskov substitution principle is the `Square` is-a `Rectangle` relationship.

In this case, a rectangle has `setHeight` and `setWidth` properties that are used to change the width and the height of the rectangle after it is constructed, as well as an `area` method.

Our Square class extends Rectangle, and overrides the setWidth and setHeight methods to ensure that the inherited width and height properties are kept in sync (since square widths and heights are always the same).

If our client code is receiving rectangles from a factory, it should be acceptable to get a square back from the factory without causing bugs in the client (since a square is-a rectangle).

If our client code calls both `setWidth` and `setHeight` on the rectangle it receives, then calls the `area` method, this could result in some odd results when the rectangle that is being operated on is a square...

That's because the square isa rectangle relationship as modeled above violates the liskov substitution principle! By exposing setWidth and setHeight as public method for our client to call, **it becomes impossible for a square subclass of rectangle to be substituted in place of a rectangle in all instances.**

If we were creating immutable rectangles instead (no public setWidth / setHeight methods), and clients asked for rectangles through a factory, then the liskov substitution principle would be satisfiable for the Rectangle and Square classes.

This was well summarized by user SteveT on Stack Overflow:

> Well, a square clearly IS a type of rectangle in the real world. Whether we can model this in our code depends on the spec. What the LSP indicates is that subtype behavior should match base type behavior as defined in the base type specification. If the rectangle base type spec says that height and width can be set independently, then LSP says that square cannot be a subtype of rectangle. If the rectangle spec says that a rectangle is immutable, then a square can be a subtype of rectangle. It's all about subtypes maintaining the behavior specified for the base type.

An additional benefit of following the liskov substitution principle is **preventing the need to change client code when new subclasses are created.**

If we can guarantee that our subclasses can be substituted polymorphically in all instances of the parent classes, by modeling our subclasses after the base spec, we can guarantee that we won't need to change the client code in reaction to the addition of new subclasses.

**This keeps client code concise and lean** since it doesn't need to change its implementation based on the type it is provided. Simple code is easy to test, and reduces the frequency of bugs.
