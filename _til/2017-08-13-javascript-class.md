---
title:  "Javascript Class Basics"
date:   2017-08-13 9:00:00
category: til
tags: [javascript, class, object-oriented, constructor, ES6]
---

TIL about the javascript `class` keyword introduced in ES6, and dug in to some of the nuances behind it.

The [`class`][class-mdn]{:target="_blank"} keyword was introduced with ES6, and with it came a whole bunch of confusion about what it actually brought to the language.

ES6 classes are used throughout React applications while creating new Components, so a firm understanding of how they function is critical to any aspiring React developer.

## Basic Classes

The `class` keyword is a clearer, easier to read syntax for creating objects and dealing with inheritance in javascript. It does NOT introduce an object-oriented inheritance model to the language: it is [syntactic sugar][sugar]{:target="_blank"} around preexisting prototype-based inheritance patterns.

The `class` keyword can be used in place of traditional function 'class' definitions used pre ES6.

For example, the following two definitions for a Base class are behaviorally identical.

{% highlight javascript %}
// traditional class constructor && prototype method definition
function BaseA(name) {
    this.name = name;
}
BaseA.prototype.greet = function () {
    return `hello ${this.name}!`;
};
{% endhighlight %}

{% highlight javascript %}
// ES6 class keyword definition of the same class above.
class BaseB {
    constructor(name) {
        this.name = name;
    }
    greet() {
        return `hello ${this.name}`;
    }
}

const baseA = new BaseA(name);
const baseB = new BaseB(name);

{% endhighlight %}

Notice that both are still invoked with the `new` keyword to create an instance of the class. The `class` keyword supports the use of an explicit `constructor` function, which should contain code to create and initialize a class instance.

Also of note, in `BaseB` I am using ES6 [`method-definition`][method-definition]{:target="_blank"} shorthand. This is sugar around the traditional `this.fnName = function() {}` when attaching methods to objects.

## Inheritance

Inheritance in javascript can be difficult to implement correctly: see my past blog post on [implementing an inheritance relationship in javascript][inheritance]{:target="_blank"} to see what I mean.

The `class` keyword provides syntactic sugar for setting up a prototypal-inheritance relationship between two classes in javascript. To state it again, **the class keyword does NOT introduce a new object-oriented form of inheritance to the language**, even though the syntax looks similar to what you might see in a traditional OO language like Java or C++. Prototypes are still at the core of inheritance-like behavior in javascript.

{% highlight javascript %}

class ChildB extends BaseB {
    constructor(name, age) {
        super(name);
        this.age = age;
    }
    greet() {
        return `${super.greet()} you are ${this.age} years old`;
    }
}

{% endhighlight %}

The `extends` keyword is used to connect a class to a parent via the prototype chain.

The `super` keyword allows you to call methods on the super-class prototype. This allows you to accomplish partial overriding behavior in your sub-classes (like in greet above). In a constructor, `super` **must be called before you reference** `this` **for the first time.**

For comparison, the following code achieves the same behavior as ChildB above, but using traditional prototype means instead of `class` sugar:

{% highlight javascript %}

function ChildA (name, age) {
    BaseA.call(this, name); // STEP 1: call parent constructor
    this.age = age;
    this.greet = function () {
        return `${BaseA.prototype.greet.call(this)} you are ${this.age} years old`;
    };
}

// STEP 2: Connect ChildA to prototype chain of BaseA.
ChildA.prototype = Object.create(BaseA.prototype);
// STEP 3: Reset constructor to ChildA's constructor.
ChildA.prototype.constructor = ChildA;

{% endhighlight %}

Whew... that's pretty confusing.

**I always forget steps 2 and 3**: connecting the new class prototype to the parent's proto chain, and resetting the constructor after doing this. Using `extends` with classes defined with the `class` keyword takes care of this for you.

Also note how `super` is no longer available, and instead we need to directly call methods on the prototype of the parent. **This couples the child implementation more tightly with the class that it extends**, creating more points of maintenance if the parent winds up changing.

## Static methods

It is also possible to declare a method as static in a class:

{% highlight javascript %}

class ChildB extends BaseB {
    // ...same as above
    static goodbye() {
        return `see you later!`
    }
}

ChildB.goodbye() // returns `see you later!`

{% endhighlight %}

Static methods do not depend on a particular instance of a class to execute, and are called directly off of the class name itself. The `goodbye()` method above does not depend on any properties that are available on a class instance (i.e. it does not reference anything off of `this` that is defined in the constructor) so it can be declared as static.

Static methods can reference other static methods using `this`.

[class-mdn]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes
[sugar]: https://en.wikipedia.org/wiki/Syntactic_sugar
[inheritance]: /til/2017-04-09-javascript-inheritance/
[method-definition]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions
