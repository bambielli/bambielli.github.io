---
title:  "’async’ script attribute"
date:   2017-04-09 10:45:00
category: til
tags: [javascript]
---

TIL about the behavior of script loading in an HTML page, when the `async` attribute is added to a script tag.


### What is the `async` attribute?

The HTML5 `async` attribute **allows a script to be downloaded and executed asynchronously from a non-inline script source** (adding `async` to an inline script does not affect behavior of its execution).

For example:

{% highlight html %}

<script src="//some-domain/a.js" async></script>
<script src="b.js" async></script>

{% endhighlight %}

These scripts will load asynchronously, and will not block the parser while they download.  

### So we should use this all the time, right? 

Not so fast... there are some considerations you'll need to make before adding `async` attributes to all of your script tags.

**The order of execution of scripts with the `async` attribute is no longer guaranteed by the script's position in the document.** In other words, in the above example it is not guaranteed that a.js will execute before b.js. This can be problematic if, for example, a.js is a library that b.js depends on to execute. 

Another consideration, is if the scripts you are asynchronously loading depend on HTML elements of the document being parsed and added to the DOM before the scripts execute.

For example, let's say script tags with the `async` attribute are included at the top of an HTML file (in the head). They will make requests for their javascript resources **without blocking the parsing of the rest of the HTML document.** In other words, the DOM will continue to be constructed while these resources are fetched. 

This is great, but as soon as the request for the javascript resource resolves, **the javascript will be immediately parsed and executed**. Since the timing of the resolution of an asynchronous request is unpredictable, it may resolve before all necessary elements of the DOM have been constructed. If your script depends on DOM elements to be present that have not yet been constructed, it will error. 

### Recommendation

If your script meets the following criteria, then feel free to add the `async` attribute and move script tags to the head of the document:

- The script does not depend on particular DOM nodes to be present to successfully execute.
- The script does not depend on the instantiation of variables or objects from other scripts to successfully execute.

Otherwise, **it is still safest to just load your scripts synchronously at the bottom of your HTML page.** 

### Addendum 

Modern browsers do a lot of tricks / performance optimization behind the scenes when they encounter script tags in a document. For example, if two scripts are included in the head, browsers will still fetch both in parallel without blocking on the execution of the first script before fetching the second. Some browsers will even continue to parse the rest of the document in the background while waiting for blocking javascript to execute, so even though a script may be render blocking it is not necessarily parser blocking. 

With this in mind, the recommendation for script loading above still stands, and the argument for using the `async` attribute while loading scripts becomes even less necessary (in my opinion).
