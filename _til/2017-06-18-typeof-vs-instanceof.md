---
title:  "instanceof vs. typeof"
date:   2017-06-18 20:25:00
category: til
tags: [javascript]
---

TIL the difference between `instanceof` and `typeof` in javascript.

I was solving a puzzle that required that I check the type of the values of the properties of an object, to see if they were strings. I thought to myself, well I'll just test the string-ness of the values using `typeof` and be done, right? WRONG!

The puzzle designer was trying to be tricky, as the input object had *both primitive strings and string Objects* as values. Calling `typeof` on a String object returns `object`. This triggered a research session in to the differences between `typeof` and `instanceof`. Find the discussion below.

### instanceof vs typeof

[Per the MDN docmentation,][typeof]{:target="_blank"} `typeof` is a *unary operator* that returns a string indicating the type of the unevaluated operand.

In the case of string primitaves and string objects, `typeof` returns the following:

{% highlight javascript %}

const a = "I'm a string primitive";
const b = new String("I'm a String Object");

typeof a; --> returns 'string'
typeof b; --> returns 'object'

{% endhighlight %}

Now let's compare to the behavior of `instanceof`.

[`instanceof`][instanceof]{:target="_blank"} is a binary operator, accepting an object and a *constructor*. It returns a boolean *indicating whether or not the object has the given constructor in its prototype chain*.

When applied to the string instances above, and compared to String, it behaves as follows:

{% highlight javascript %}

const a = "I'm a string primitive";
const b = new String("I'm a String Object");

a instanceof String; --> returns false
b instanceof String; --> returns true

{% endhighlight %}

### How to properly test for strings?

In the case of the puzzle I was trying to solve, I had to detect both primitive strings and String objects as values of the object in question's keys.

To account for both primitives and objects, I came up with the following function:

{% highlight javascript %}

const isString = (str) => {
  return typeof str === 'string' || str instanceof String
}

{% endhighlight %}

This example provided clarity on the differences between `typeof` and `instanceof` in javascript.

[typeof]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof
[instanceof]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof
