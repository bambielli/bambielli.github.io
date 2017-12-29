---
title:  "SOLID: Interface Segregation Principle"
date:   2017-12-29 11:23:00
category: til
tags: ["interface", "segregation", "principle", "interface segregation principle", "interface-segregation-principle", "architecture", "design", "software"]
---

TIL about the interface segregation principle, and how it promotes decoupling of classes in your code through the separation of methods in to `role interfaces`.

The `interface segregation principle` is the `I` in SOL**I**D, and as the name suggests it is concerned with the design of interfaces in your application.

The principle states that **no client should be forced to depend on methods it does not use**.

In complicated applications, it can be easy to lump a bunch of methods into a common interface that many clients use. This leads to tight coupling of clients to code that has no relevance to their function.

Consider a `Duck` interface. It contains methods for quacking and eating (as ducks do).

{% highlight java %}

public interface Duck {
  public void quack();
  public void eat();
}

{% endhighlight %}

Now consider implementing a few concrete Duck classes, like `MallardDuck` and `RubberDuck`, that both implement the `Duck` interface.

{% highlight java %}

public class MallardDuck implements Duck {
  @Override
  public void quack() { // quacking logic };
  @Override
  public void eat() { // eating logic };
}

public class RubberDuck implements Duck {
  @Override
  public void quack() { // quacking logic };
  @Override
  public void eat() { // eating logic??????? };
}

{% endhighlight %}

Is it clear that we have an interface segregation problem here? A `RubberDuck` doesn't eat, but it is required to implement the eat method of the Duck interface which is irrelevant to its operation.

This creates unnecessary coupling between the eat method and classes that implement the interface which do not require eating. If the eat method changes in the interface, we will need to update all classes that implement Duck. This leads to more changes in our codebase, more testing for regressions, and slower deploy times.

The interface segregation principle would tell us to separate the `eat` and `quack` methods of the `Duck` interface in to two different interfaces like so:

{% highlight java %}

public interface Eatable {
  public void eat();
}

public interface Quackable {
  public void quacks();
}

{% endhighlight %}

Now `MallardDuck` can implement both `Eatable` and `Quackable`, while RubberDuck can just implement `Quackable`. This only couples the `RubberDuck` to the method that is relevant to it. If the definition of Eatable changes, we would only need to update `MallardDuck` and could leave `RubberDuck` alone!

Following the interface segregation principle strictly can lead to a ton of different `role specific interfaces` in our code, which is definitely a tradeoff in readability / verbosity.

To me, it seems like the interface segregation has similar goals as the single responsibility principle, but in regards to interfaces instead of classes.

Interfaces should define a single `role` that an object can play in your code. Designing a system in this way will isolate changes to the role to just the relevant objects, reducing the scope and potential impact of changes to the role.
