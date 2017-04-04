---
title:  "Design Patterns: Decorator"
date:   2017-04-02 14:19:00
category: post
tags: [design-patterns, composition]
---

Next up in my Design Patterns series is the `Decorator` pattern, which allows you to give your objects new responsibilities at runtime without making any code changes to their underlying classes.

[Find my last Design Patterns entry on the Observer pattern here.][observer]{:target="_blank"}

### Favor Composition over Inheritance

This is an oft mentioned phrase when learning Object Oriented Programming, and it surfaces again when studying the utility of the decorator pattern.

When sub-classing, behaviors inherited from the parent are statically set at compile time. This leads to valuable compile time checks and *can* reduce the frequency of runtime errors, but it also introduces rigidity to your class hierarchy.

Dynamically extending an object's behavior through composition can be done at runtime, and removes the rigidity introduced by inheritance. In addition, it allows you as the developer to write *new code to extend the target object with new behaviors* instead of needing to alter the original class code. Writing new code eliminates the potential for introducing bugs in the parent class, or inadvertently causing side-effects in code elsewhere in the application that relies on the original class definition.

This hits at a core principle of good class design called the "Open/Closed Principle": **classes should be open for extension, but closed for modification.**

### The Decorator Pattern

The Decorator pattern allows us to "decorate" (extend) objects with added functionality at runtime. It gives developers the power to **compose objects that are purpose driven for the current demands of their users**.

Decorators can be thought of as wrappers around the object they are decorating: they are the same type as the object being decorated (SuperType), and therefore the decorated object can be passed and used in a program in place of the original wrapped object. See class diagram below:

<figure>
  <img src="/assets/images/DecoratorClassDiagram.jpg">
  <figcaption>Class Diagram for Decorator Pattern</figcaption>
</figure>

 The diagram above shows how the decorator pattern might be implemented in a traditional OO language like Java.

 - `Component` and `Decorator` themselves are both either abstract classes or interfaces (doesn't matter which).
 - `Decorator` extends (or implements) `Component`, which gives concrete implementations of `Decorator` the ability to be used in place of concrete implementations of `Component`!
 - Each concrete implementation of `Decorator` has a field of type `Component` (the SuperType shared between the decorator and the decorator target) where the instance of the object `ConcreteComponent` to be decorated is set.
 - Methods overridden in a `Decorator` will delegate their implementation to the object they are wrapping, then will extend or modify that implementation with new behavior.
 - Since the concrete `Decorators` themselves share the type `Component`, decorators can be wrapped in other decorators! Each one adds a new layer or "wrapper" to the inner concrete object.
 - `Decorator` is useful for *extending* behaviors exposed as abstract methods by the shared supertype (`Component` above), but cannot be used to add brand new behaviors to objects that do not already exist as abstract methods in the shared supertype. In other words, a decorator cannot add a brand new method to a decorated object, because the contract doesn't exist in the shared abstract supertype.

### Pizza Shop Example

Let's say we have a pizza shop: pizzas can have various pizza toppings like Pepperoni or Cheese, and can be either Thin Crust or Deep Dish (I am from Chicago after all!). [Check out the following repository with some test code that I wrote][test]{:target="_blank"} to get an idea of how the decorator pattern can help design a system that flexibly meets any pizza / topping combination the customer throws at you!

<figure>
  <img src="/assets/images/PizzaClassDiagram.jpg">
  <figcaption>Class Diagram for Pizza Shop</figcaption>
</figure>

### Downsides of Decorator

There are several limitations of the `Decorator` pattern that I'd like to highlight:

- If you have code that relies on **the type of the concrete implementation that you are wrapping** (i.e. DeepDish or ThinCrust from the pizza example above) then decorator will obscure that type information from you.
- Decorating your objects manually **is a pain**. Lots of parameters to remember to pass and many constructors if everything is a new object. Combining `Decorator` with the `Factory` or `Builder` patterns alleviates some of this overhead, and makes building purpose driven objects much simpler and less prone to error.
- As seen from my example repo, using `Decorator` often leads to **numerous small classes** that are each responsible for one behavior modification. This makes the code somewhat more difficult to read and understand.

### Conclusion

Doing a deep dive in to the Decorator pattern helped me get a better understanding of its pros and cons. The Open/Closed principle (keeping closed parts isolated from new extensions) is great to strive for while developing systems, and `Decorator` gets us closer to this goal.

My next post will be about the Factory pattern, coming soon!

[observer]: /posts/2016-10-25-observer/
[test]: https://github.com/bambielli/DecoratorExample
