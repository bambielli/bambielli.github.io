---
title:  "Spread Operator Does Not Deep Copy Properties"
date:   2017-01-29 20:58:00
category: til
tags: [javascript]
---

TIL that the spread operator does not perform a deep copy of object properties to the target object.

This led to a long debugging session at work that had me thinking I was crazy for a while!

### The Spread Operator vs Object.assign()

The [spread operator][spread]{:target="_blank"} (written as `...`) can be used to assign an object's enumerable properties to a new object. It is often used to replace the use of `Object.assign()` since it is more succinct to write, and is suggested for use when writing Redux code by the Redux documentation. See the following example for a comparison of the spread operator and `Object.assign()` when creating a new object:

{% highlight javascript %}
  const obj = {a: 1, b: 2};
  const obj2 = {c: 3};

  const assignedObject = Object.assign({}, obj, obj2);

  const spreadObj = {
    ...obj,
    ...obj2
  }
  console.log(assignedObject) // this logs {a: 1, b: 2, c: 3}
  console.log(spreadObj) // this logs {a: 1, b: 2, c: 3}

  // both result in the same object
{% endhighlight %} 

Spread, to me, is much more succinct, and since it is suggested for use by the [Redux documentation][redux]{:target="_blank"}, we use the spread operator on objects liberally throughout our project. I'll also acknowledge that since it is still a Javascript stage-3 feature, that it is still somewhat risky to use.

### My Problem

I was working with an object that contained properties with values that were themselves objects. Something like this: `{a: {b: 1}}`.

When I wanted to make a change to the state of this object, I used the spread operator to create what I considered a "new object" with the properties of my source. Then, I would change the state of a sub key of this copied object (i.e. I would change the value of property `b` above from 1 to 2.

Since I thought I was making an isolated "deep" copy of the original object, I thought these mutations would be isolated from the original... but they were not! An example for clarity:

{% highlight javascript %}

const original = {a: {b: 1}};

const falseCopy = {...original};

falseCopy.a.b = 2; 

console.log(falseCopy) // logs {a: {b: 2}}
console.log(original) // also logs {a: {b: 2}} WTF!

{% endhighlight %} 

Under the hood, Babel is transpiling the `...` to `Object.assign` (it contains a fallback if used in a browser where `Object.assign()` is not yet implemented). From the [MDN][mdn]{:target="_blank"} documentation: _The `Object.assign()` method is used to copy the values of all enumerable own properties from one or more source objects to a target object. It will return the target object._

In retrospect, I should have realized that a "copy" is nothing more than a shallow copy, and in the case of reference types just a copy to the reference. This caused some weird bugs where I was anticipating changes made to the copy to be isolated, but I eventually tracked down the cause (and learned something in the process).

[spread]: https://github.com/sebmarkbage/ecmascript-rest-spread#spread-properties
[redux]: http://redux.js.org/docs/recipes/UsingObjectSpreadOperator.html
[mdn]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
