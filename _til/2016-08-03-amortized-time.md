---
title:  "Amortized Time Complexity"
date:   2016-08-03 22:03:00
category: til
tags: [systems]
---

TIL that the amortized time complexity of adding an item to an ArrayList in Java is O(1), but that the "worst case" for an add operation is O(n). This caused me to look up a few SO posts on what exactly is meant by "amortized time."

## What is it?

Amortized time is time taken to perform an operation, averaged over a large number of repetitions of that operation.

In terms of `ArrayList.add`, an item can be added to an array list in O(1) time **except** in the case that the array is full, at which point the size of the array is doubled and re-allocated at a different point in memory and old items are copied over into the new array. The old array is then free'ed, and the new "double size" array is the array to which any references now point. This re-allocation process takes `O(n)` time, where `n` is the current size of the array.

This process happens less and less frequently as the size of the array grows, though, since the array's size is doubled in each re-allocation step. The cost of re-allocation can be distributed amongst other insertion events, making the time complexity essentially a constant factor. `ArrayList.add` therefore has **constant amortized time complexity** even though its worst case is O(n).

