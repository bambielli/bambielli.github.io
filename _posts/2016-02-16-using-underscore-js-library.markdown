---
layout: post
title:  'Underscore.js'
date:   2016-02-16 19:53:00
category: til
tags: [javascript, underscore-js]
---

TIL about Underscore.js (an awesome javascript util library) that prevented me from needing to write a custom dictionary-parsing method.

Today in angular service land, I needed a way to extract the values out of a dictionary.

The dictionary in question keeps track of paged data that has been returned from the backend. The service I'm writing will first check this dictionary to see if data for the required pages already exists (has already been fetched) and if so, will just fetch it from the dictionary instead of making another request to the server.

The service can be polled to return multiple pages of data, and if so, I wanted to be able to extract the appropriate pages out of the dictionary (dictionary is indexed by page number) and concatenate the values together into an array that is returned to the caller. This is a fairly simple task, but I wanted to make sure I wasn't re-implementing something that has, I'm sure, been implemented countless times before.

I was about to write a method that did this data massaging, when I remembered we import [Underscore.js][underscore]{:target="_blank"} in our app! Underscore.js is a fully tested javascript library that contains a ton of functional programming and util methods for manipulating javascript objects.

After some digging through the docs, I found a method that did exactly what I wanted:

__Example:__

{% highlight javascript %}

var obj1 = {"val": "hello", "val2": "world", "val3": "!"};

//desired output ["hello", "world", "!"]

var output = _.values(obj1); //the _ stands for "underscore"

console.log(output); //logs ["hello", "world", "!"]

{% endhighlight %}

__This made my day!__

It was so easy, and just required looking through the underscore docs to see what was available.

I'll definitely be leveraging more underscore methods in the future.

[underscore]:  http://underscorejs.org/
