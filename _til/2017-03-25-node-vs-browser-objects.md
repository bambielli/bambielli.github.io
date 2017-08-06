---
title:  "node globals vs browser globals"
date:   2017-03-25 15:35:00
category: til
tags: [node, javascript]
---

TIL that node programs have different globally accessible objects than those found in the browser.

## Globals in the browser

In the browser, we have access to two global objects: `window` and `document`

`window` is the highest level scope of the javascript scope heirarchy in the browser. If you declare a global variable in javascript, it is attached as a property of window. Window also contains all globally accessible browser APIs that the browser initializes by default e.g. `localStorage` and `console`.

Example:

{%highlight javascript%}
 var a = 'global string'; //this string is attached to `window`

 window.a; // returns 'global string'

{%endhighlight%}

`document` is an object that points to the highest parent node of your currently visible document object model (DOM). Since it is a globally accessible object created by the browser, it is also a property on window.

Example:

{%highlight javascript%}

document; // returns an object that contains all nodes in currently visible DOM

window.document; // returns the same object as the line above!

{%endhighlight%}

Requesting `childNodes`from `document` returns an array of 1 element: the outermost HTML tag of the currently visible DOM.

## Node equivalents

The equivalent objects in a `node` program are named `global` and `process`.

Since there isn't a browser `window` in a `node` program (code can execute anywhere, not just in the browser) the highest-level scope in your node program is called `global`. It can be interacted with in the same way as the `window` object, and like `window` it also contains globally accessible objects and methods for your node program (e.g. `setTimeout` and `console`). You may also reference these global properties without explicitly calling them from `global`.

Example:

{%highlight javascript%}

var a = 'global string';

global.a // returns 'global string'

global.console.log('hi') //logs 'hi'

console.log('hi') //also logs 'hi'

{%endhighlight%}

Similarly, since code is not executing in the browser, there is no DOM available to a `node` process. There are, however, APIs that `node` exposes to interact with the running node process. `node` exposes these APIs to the user in a second global object called `process`.

The objects and APIs available from `process` give you access to details about the environment in which the node process is running.

{%highlight javascript%}

process.env  //returns an object with environment variables set for the execution environment
process.memoryUsage() //returns data about the memory being consumed to run the process

{%endhighlight%}

Since `node` offers a different execution environments for your javascript, it makes sense that the global objects are named differently than the browser equivalents. I've used `process.env` many times to grab environment variables from my execution environment, but I never took time to look at what else was exposed to a program via process (like `exit()` or `cpuUsage()`).
