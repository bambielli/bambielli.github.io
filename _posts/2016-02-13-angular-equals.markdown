---
layout: post
title:  "angular.equals for JSON Object Comparison"
date:   2016-02-13 16:40:00
category: til
tags: [angular, javascript, json]
---

TIL that angular has an "equals" method that does a deep comparison between objects to determine equality.

__Example:__

{% highlight javascript %}

var object1 = {'1': 'hello', '2': 100};
var object2 = {'2': 100, '1': 'hello'};

object1 === object2; // evaluates to false

angular.equals(object1, object2); // evaluates to true

{% endhighlight %}

I needed to do a deep compare between two JSON objects to see if they contained equivalent values, and was about to write my own deep compare when I stumbled upon this built-in. Way to go angular!

