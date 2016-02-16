---
layout: post
title:  'Javascript - Call by Sharing?'
date:   2016-02-15 20:13:00
category: til
tags: [javascript, programming-languages]
---

TIL that there is a programming language evaluation strategy called "call by sharing," and that Javascript implements this approach.

As I understand it, __call by sharing is a hybrid of the "call by value" and "call by reference" evaluation strategies__ that are implemented by other languages.

When a parameter is passed to a function in Javascript, that parameter __cannot be reassigned__ to a new object or value within the scope of the called function: the original reference or value remains with the caller. A parameter, however, __can be mutated__ within a called function's scope (if the object is mutable) and the caller will see this mutation.

__Example [1]:__

{% highlight javascript %}

var i = 25;
var obj1 = {val: "hello"};
var obj2 = {val: "hello"};

var update = function (j, o1, o2) {
	j = 200;
	o1.val = "goodbye";
	o2 = {val: "goodbye"};
}

update(i, obj1, obj2);

console.log(i); // remains as 25
console.log(obj1.val); // changed to "goodbye"
console.log(obj2.val); // remains as "hello"

{% endhighlight %}

If Javascript was purely call by value, then the mutation of obj1 would not be seen by the caller. If it was purely call by reference, then all three of the updates from the function above would have been seen by the caller. Call by sharing truly seems to be a hybrid of the two more commonly referenced evaluation approaches.

The name "call by sharing" is not colloquially used to describe this evaluation strategy. Java, Python and Ruby all implement call by sharing, but members of those communities do not reference it by its proper name. This inconsistency has led to confusion among users of multiple languages (myself included) and I'm glad I finally took some time to understand and sort out the differences.

Related Links:

 - [1] [http://stackoverflow.com/questions/518000/is-javascript-a-pass-by-reference-or-pass-by-value-language](http://stackoverflow.com/questions/518000/is-javascript-a-pass-by-reference-or-pass-by-value-language){:target="_blank"}