---
layout: post
title:  "Synchronous Javascript"
date:   2016-03-16 22:16:00
category: til
tags: [javascript]
---

TIL that javascript is really a **synchronous** language with callback capabilities that make it feel asynchronous at times.

I found [this video][video]{:target="_blank"} from a javascript conference about a year ago, and it really helped with my understanding of what is going on under the hood in the browser when javascript is being executed.

### JS is a synchronous language...

Javascript is a **synchronous** language, meaning it does not have multi-threading capabilities and if you are executing a chunk of javascript it blocks any other activity from occurring on the page. As a script is executed, method calls are placed on the call stack of the browser's JS runtime (V8 in Chrome), and are popped off when either the method's return is reached or the end of the method definition is reached.

### ...but it has the ability to act in asynchronous ways

But Brian, What about Ajax calls? What about functions wrapped in a setTimeout? A good observation!

Methods like these accept callbacks that are deferred until the underlying method (a server request, or a timer finishing) returns successfully or unsuccessfully. The callbacks from these deferred methods are added to the browser's task queue once they have resolved. ***Once the call stack from the original script being executed has been completely finished*** (the call stack is empty) the browser's ***event loop*** will begin adding methods from the task queue to the call stack to be executed. Methods are processed in a FIFO order in the task queue.

### Visualizing method execution

The speaker in the video linked above created a tool to help visualize the order of execution of statements in various deferred and synchronous method calls.

Check it out [here][loupe]{:target="_blank"}!

***Extra TIL:*** Say you want to defer the execution of a method until all other methods have finished executing...it is possible to wrap such a method in a setTimeout ***with a timeout of 0.*** This tells the browser to defer execution of the wrapped method until all other non-deferred (synchronous) methods have been processed.


[video]: https://www.youtube.com/watch?v=8aGhZQkoFbQ
[loupe]: http://latentflip.com/loupe/
