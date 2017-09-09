---
title:  "Design Patterns: Adapter and Facade"
date:   2017-09-09 11:35:00
category: post
tags: [presentation, design-patterns, adapter, facade]
---

Last week, as part of Expedia Learniversity, I gave a presentation on the Adapter and Facade design patterns.

## Adapter: Interface with Anything

The Adapter pattern is useful when you are trying to integrate components of your system that have incompatible interfaces. They perform a conversion of a target interface (the interface a client expects) to the adaptee's interface.

The canonical example of an adapter is the **US plug to Euro socket adapter.** This adapter accepts a US plug (client) and provides (implements) a US socket interface for it to integrate with (target interface). The adapter exposes a euro plug on the other side, which can be plugged in to any euro socket (the adaptee).

Internally the adapter performs necessary conversions between what a device with a US plug would expect from its power source, and what a euro socket provides. **This additional conversion work is invisible to the client**, as it just sees a US socket interface that it can integrate with, and nothing more.

Speaking in software terms, an adapter class **implements the target interface that the client expects**, and **composes an instance of the adaptee object** which it uses internally to perform conversion from target to adaptee.

See the following implementation of an adapter that implements a java Iterator interface as its target, and composes with a java Enumeration object as its adaptee.

{% highlight java %}

import java.util.Enumeration;
import java.util.Iterator;

public class IteratorToEnumerationAdapter implements Iterator {
    Enumeration enumeration;

    IteratorToEnumerationAdapter(Enumeration enumeration) {
        this.enumeration = enumeration;
    }

    @Override
    public boolean hasNext() {
        return enumeration.hasMoreElements();
    }

    @Override
    public Object next() {
        return enumeration.nextElement();
    }

    @Override
    public void remove() {
        throw new UnsupportedOperationException("Remove doesn't exist");
    }
}

{% endhighlight %}

By wrapping an instance of Enumeration, our client code can use this adapter to interface with Enumerations as if they were Iterators! Since the adapter implements Iterator, it is also of Iterator type! This means the adapter can be used in place of Iterator anywhere in our client code where Iterator is expected.

Notice that the Iterator interface exposes a 3rd method `remove()` that is unsupported by an enumeration object. An option in this scenario might be to implement the missing behavior in our adapter class. This somewhat deviates from the definition of Adapter, though, since **a pure adapter is just supposed to convert from one interface to another without adding any additional behavior.**

In the example above, we choose to throw an `UnsupportedOperationException`, which indicates to our client that the Iterator they are interfacing does not support `remove()`.

## Facade: Simplify your Subsystem Interfaces

Consider a typical home theater system: it contains many different components like Screens, Lights, Receivers, Projectors, and maybe even popcorn poppers.

When a client comes along and wants to watch a movie, **they need to know both the order and the operations to perform on each component of the subsystem to achieve their goal**. For many, this can be a daunting task, and gets even worse when the home theater nerd starts upgrading components that expose different methods and **break other clients understandings of how the system works.**

Facade to the rescue: a Facade is a simplified interface that sits on top of a system, that exposes methods necessary for clients to achieve their goals. In the case of the home theater, a universal remote acts as a facade. Movie watching clients can use the simplified interface that the universal remote provides, push one button, and have all of the necessary components turn on in the correct way.

The facade **does not encapsulate subsystem components away from clients**. The nerds can still come in and have full control over the different components of the home theater system, if they so choose.

An advantage of facade is that **it decouples clients from system components**. This makes upgrades easier to perform: when a component changes underneath the facade, it should make sure to update the facade appropriately. This will ensure that clients get the same experience regardless of the underlying component architecture.


## Presentation

I gave the following presentation on Adapter and Facade at our weekly Learniversity session at work. [If you are on mobile, click here for a PDF of the presentation.][here]

<embed src="/assets/pdf/Adapter-Presentation-09-09-17.pdf" />

[here]: /assets/pdf/Adapter-Presentation-09-09-17.pdf
