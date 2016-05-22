---
title:  "Sorting Arrays in Javascript"
date:   2016-03-07 17:02:00
category: til
tags: [javascript]
---

TIL how the `Array.prototype.sort()` sorts by default, and how to pass a comparator function in to the sort method if a different sort order is desired.

I needed to sort an array of integers today using javascript. I knew arrays had a sort method baked in to the prototype, so I figured this would be well suited for sorting my integer array...WRONG! See below.

{%highlight javascript%}
var arr = [20, 3000, 3, 100];
arr.sort();

//the array is now sorted as follows
// [100, 20, 3, 3000]
{%endhighlight%}

***What is going on???***

`Array.prototype.sort()` *default sorts by ascii value.* My integers were being converted to strings and compared as such. In the case above, integers with a 1 as their first digit come first in ascii...then 2...then 3...

To do a proper **integer sort**, you need to pass a **comparator function** to the `Array.prototype.sort()` method that dictates how sorting should be accomplished. The comparator takes 2 arguments `(a, b)` which represents two adjacent elements in the array to be sorted.

  1. If **0 is returned** from the comparator, nothing is done to the positions of 'a' and 'b' in the array
  2. If a value **greater than 0 is returned**, 'b' comes first positionally in the array relative to 'a'
  3. If a value **less than 0 is returned**, 'a' comes first positionally in the array relative to 'b'

**A proper integer sort is achieved by the following:**

{%highlight javascript%}
var arr = [20, 3000, 3, 100];
arr.sort(function(a, b){
	return a - b;
});

//the array is now sorted as follows
// [3, 20, 100, 3000]
{%endhighlight%}

The comparator function above returns values that comply to the 3 rules above, and will sort the integer values appropriately.

See the following reference from [Mozilla][mozilla]{:target="_blank"} on `Array.prototype.sort()` for additional info.

[mozilla]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
