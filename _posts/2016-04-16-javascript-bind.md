---
layout: post
title:  "How to use Bind in javaScript"
date:   2016-04-16 04:01:00
category: til
tags: [javascript]
---

TIL about the `bind` method in javaScript and how to use it.

Bind is used for "binding" another object context to a method call. In other words, you can swap out the object that `this` references in a method by calling bind on the method and passing in a context.

###Example 1:
Let's look at the following example (not using bind yet):

{%highlight javascript%}

var Brian = {
	firstName: 'Brian',
	lastName: 'Ambielli',
	fullName: function() {
		return this.firstName + ' ' + this.lastName;
	}
};

var fName = Brian.fullName; //this assigns the object's method to the global variable fName

console.log(fName()); //this will print 'undefined undefined'

{%endhighlight%}

The result of the console.log is 'undefined undefined'! Why is that? Because we "borrowed" the method from the `Brian` object, but we did not borrow its context! Context is what the `this` in a method refers to, and it is defined by default as the object in which the method is invoked.

A call to `Brian.fullName()` will call the `fullName` method with the Brian context, and will appropriately return 'Brian Ambielli'. However, when calling the method in the global scope (as we do when calling `fName()` above) the context is the global "window" object.

###Example 2:

{%highlight javascript%}
var Brian = {
	firstName: 'Brian',
	lastName: 'Ambielli',
	fullName: function() {
		return this.firstName + ' ' + this.lastName;
	}
};

var fName = Brian.fullName; //this assigns the object's method to the global variable fName

var firstName = 'Jim';
var lastName = 'Frank';

console.log(fName()); //this will print 'Jim Frank'

{%endhighlight%}

In this case, the global object has `firstName` and `lastName` properties attached to it already, so when the function references `this.firstName` and `this.lastName` those properties will actually be defined (on window). But what if we just want to call `fName` global scope with the context of the borrowed method's original object?

This is where `bind` comes in.

###Example 3:

{%highlight javascript%}
var Brian = {
	firstName: 'Brian',
	lastName: 'Ambielli',
	fullName: function() {
		return this.firstName + ' ' + this.lastName;
	}
};

var fName = Brian.fullName.bind(Brian); //this binds the Brian context to the fullName method

var firstName = 'Jim';
var lastName = 'Frank';

console.log(fName()); //this will print 'Brian Ambielli'

{%endhighlight%}

We did it! By binding the object `Brian` to the `fullName` method when we assigned it to the global variable `fName`, we were able to preserve the context for that copy of the method.

Bind is useful when we want to permanently "borrow" methods from other objects, and specify the context in which we want the copy to operate.

If we do not want to permanently "borrow" a method (assigning the copy to a new property in our intended scope) javaScript has other ways to `call` or `apply` methods in place without needing to make a copy.

For another day...

###Extra Example:

Apparently it is possible to bind and invoke the bound function in place.

{%highlight javascript%}
var Brian = {
	firstName: 'Brian',
	lastName: 'Ambielli',
	fullName: function() {
		return this.firstName + ' ' + this.lastName;
	}
};

var fName = Brian.fullName;

var firstName = 'Jim';
var lastName = 'Frank';

console.log(fName.bind(Brian)()); //this will print 'Brian Ambielli'

{%endhighlight%}

The last line binds fName to the Brian context, and then invokes it immediately. This effectively mimics what `call` does. Not sure if this is at all good practice, and I feel like using `call` is preferred. EDIT: According to SO it isn't "bad" practice per-say...you are basically doing the same thing that `call` does.

