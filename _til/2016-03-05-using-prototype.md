---
title:  "Using Javascript Object Prototypes"
date:   2016-03-05 19:13:00
category: til
tags: [javascript, performance, efficiency, design-patterns]
---

TIL how to use javascript object prototypes, to associate common methods and data types with the "superclass" for a javascript object.

Javascript is a __prototype-based language__, meaning instead of defining explicit class inheritance pattern in a hierarchy you are able to extend and decorate object `prototypes` for each type of object created. All objects inherit from the generic `Object` prototype (this is similar to how all objects inherit from "Object" in java), and new object types also define their own object prototype.

It is best practice to place common object method definitions in the prototype, since definitions are shared between all instances of the object. __If method definitions are defined in the object constructor, a new copy of the method definition is stored in memory for each constructed object__ (which is highly inefficient). Storing at the prototype level only defines the method once.

Object prototypes have access to any data fields defined for the object.

{%highlight javascript%}
var Car = function(gas) {
   this.gas = gas;
}

Car.prototype.drive = function() {
   this.gas--;
}

var c1 = new Car(1);
c1.drive(); // decrements gas by 1.
console.log(c1.gas) // prints 0;
{%endhighlight%}


