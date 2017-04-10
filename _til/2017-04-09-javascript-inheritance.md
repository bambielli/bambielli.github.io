---
title:  "Prototypal Inheritance"
date:   2017-04-09 17:09:00
category: til
tags: [javascript, object-oriented]
---

TIL how to implement an inheritance relationship (from scratch) in javascript using `prototypal inheritance`.

During class this week, we went over constructors and prototypes in the context of javascript. The students were asking great questions about the extensibility of the javascript prototype and how to implement inheritance hierarchies like in other OO languages. I found myself a bit rusty in the subject, so I did some searching after class and stumbled upon [this article from MDN][tldr]{:target="_blank"} which does a good job at explaining the subject.

I decided to take my own crack at explaining how to implement an inheritance relationship in javascript! Read on to learn more.

### What is prototypal inheritance?

Javascript has a different system for implementing inheritance hierarchies than those used by traditional Object Oriented languages like C++ and Java: prototypal inheritance!

Unlike C++ and Java, javascript objects do not have inherited functionality *copied over* to the inheriting object from the parent. Instead, functionality is linked to the inheriting object via the `prototype chain`, which, in essence, defines the set of objects from which a particular object inherits. **The linking of functionality via the `prototype chain` is the javascript equivalent to the inheritance systems found in traditional OO class designs.** 

It is worth noting that an object can only have one prototype, and an effect of this is that javascript only supports single inheritance (like Java).

### Prototypal Inheritance in Action: Pie Hierarchy (peirarchy?)

Let's see what prototypal inheritance looks like in code. The following is a constructor for a Pie: Pies have a size and toppings to add to each pie.

{% highlight javascript %}
  
function Pie (size, toppings) {
  this.size = size
  this.toppings = toppings
}

Pie.prototype.getDescription = function () {
  console.log(`Pie is ${this.size} inches large,
    and has the following toppings: ${this.toppings.join(', ')}`)
}

{% endhighlight %}

Notice that I put the `getDescription` method on the Pie prototype. Assigning a method directly to the prototype **ensures that the method is not re-defined by each instance of Pie**, and will be used by any object that exists on the prototype chain below Pie.

### It's Pizza (subclass) Time!

Now I will create a new object constructor for a Pizza. A Pizza `is-a` Pie, so it should inherit from and subclass pie. In java we would write `public class Pizza extends Pie` to implement this type of relationship, but in javascript we do the following:

{% highlight javascript %}
  
  function Pizza(size, toppings, isDeepDish) {
    Pie.call(this, size, toppings)
    this.isDeepDish = isDeepDish
  }

{% endhighlight %}

The constructor looks similar to Pie, adds a new field called `isDeepDish` that reflects if the pizza is Chicago style (this should always be true IMO...) but also uses the [`call()`][call]{:target="_blank"} method to execute the Pie constructor. `Pie.call(this, size, toppings)` is the javascript equivalent of calling `super(properties)` inside of a sub-class constructor in Java.

`call` allows you to pass a different execution context to a function that is defined elsewhere in your program. In the Pizza constructor above, we pass the Pizza constructor's `this` as the execution context to the Pie constructor, plus the relevant arguments necessary for that constructor. **This results in `this.size` and `this.toppings` being defined for new Pizza objects, even though those properties are defined as part of the Pie constructor.**

### Completing the Prototype Link

In the above example, **the prototype of Pizza is not yet linked to the prototype chain that contains Pie.** In current state the pizza prototype just links to the Pizza constructor itself. This means that since the `getDescription` method is defined on the Pie prototype, the method is not available to Pizza objects and will result in a 'method is not a function' error if it is called on a new Pizza object.

We need to link the Pizza prototype to Pie so we inherit the methods defined on the Pie's prototype.

*Side Note: this behavior only applies to methods that are defined on the parent's prototype, not to methods that are defined inside the parent's constructor.*

*For example, if `getDescription` was defined as a property on `this` inside of the Pie constructor, then when we executed `Pie.call()` inside of the Pizza constructor the method would have been re-defined on the new Pizza object.*

The following example shows how to link the Pie prototype to Pizza.

{% highlight javascript %}
  Pizza.prototype = Object.create(Pie.prototype)
  Pizza.prototype.constructor = Pizza
{% endhighlight %}

The first line sets the prototype of pizza to a new instance of the object stored in `Pie.prototype`, which accomplishes the linking of the Pizza prototype to the Pie chain! This means we now have access to any methods that are defined on the Pie prototype in our Pizza objects.

However, a side effect of overwriting the Pizza prototype with a copy of Pie's is that **we also overwrite the constructor in `Pizza.prototype.constructor` with a pointer to Pie's constructor.** The second line fixes this problem by resetting the `Pizza.prototype.constructor` to the original Pizza constructor.

Since we used `Object.create()` to set our `Pizza.prototype`, the Pie prototype will be unaffected by any additions we add to `Pizza.prototype`. **That's because `Object.create()` deepCopies and returns a brand new instance of the object it is passed,** thus isolating the `Pizza.prototype` from the `Pie.prototype`.

We can confirm the linked-nes of our constructor prototypes by calling `isPrototypeOf()` on our object instances:

{% highlight javascript %}
  console.log(Pizza.prototype.isPrototypeOf(pizza)) // should log true
  console.log(Pie.prototype.isPrototypeOf(pizza)) // should log true
  console.log(Pizza.prototype.isPrototypeOf(pie)) // should log false
{% endhighlight %}

### Overriding Methods on the Chain

Both overriding and partial overriding of methods is possible in javascript as well.

To completely override a method on the prototype chain, just define a new method with the same name on the child prototype.

{% highlight javascript %}
  Pizza.prototype.getDescription = function () {
    console.log(`This pizza is a deepdish ${this.deepDish}`)
  }
{% endhighlight %}

Calling the method above on a pizza object will not delegate farther up the prototype chain, since a method named `getDescription` is now defined at the Pizza level.

Partial overriding of parent methods is also possible by referencing the method on the parent prototype from inside the body of the override:

{% highlight javascript %}
  Pizza.prototype.getDescription = function () {
    Pie.prototype.getDescription.call(this) // note the `.call(this)`
    console.log(`This pizza is a deepdish ${this.deepDish}`)
  }
{% endhighlight %}

The `call(this)` is important, because without it `Pie.prototype.getDescription` will not have the correct execution context. We pass the execution context `this` which in our case will be an instance of a Pizza object.

### Final thoughts

It was a useful exercise to implement a prototypal inheritance relationship from scratch in javascript. In summary, here are the steps necessary to go about creating a subclass that inherits from a parent constructor:

1) Define a logical subClass constructor, and inside of it call the desired superClass constructor: `Pie.call(this, size, toppings)`.

2) Set the prototype of the subClass to a deep copy of the superClass's prototype: `Pizza.prototype = Object.create(Pie.prototype)`

3) Reset the subClass's constructor after overwriting the original in step 2: `Pizza.prototype.constructor = Pizza`

I learned a ton from this exercise, and I hope you did too! You can find a file with all of the code samples I go over in this blog post, plus some additional commentary, [in my github.][gh]{:target="_blank"}

[tldr]: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance
[call]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call
[gh]: https://github.com/bambielli/jspractice/blob/master/constructorExample.js
