---
title:  "XMLHttpRequest Example"
date:   2017-07-04 11:34:00
category: til
tags: [javascript, browser, ajax]
---

TIL how to initiate a request using native browser `XMLHttpRequest` objects, instead of a request library that wraps these objects.

I've always used libraries like `fetch` or `jQuery.ajax()` to make AJAX requests from my front end applications. Since these libraries just wrap XMLHttpRequest objects, I figured it would be a good exercise to figure out how these requests are made.

I created a small proof of concept project that initiates a request using XMLHttpRequest objects. [Find it here][here]{:target="_blank"}, and be sure to take a look at the [javascript I wrote][wrote] to make the request happen (it is well commented).

Takeaways from this exercise:

- Internet Explorer has its own version of XMLHttpRequest called `ActiveXObject`. If you want your requests to work in IE, it is necessary to first test to see if ActiveXObject is attached to window, then use that to construct your xhr object instead of `XMLHttpRequest`.

- 5 states of an XMLHttpRequest: 0) Client Created, request has not been made 1) xhr.open() has been called 2) xhr.sent() has been called, and headers are available from the response 3) xhr.responseText is downloading 4) Request is complete!

- You need to attach an `onreadystatechange` listener to your xhr object, which will be called every time the readystate changes. Once `readystate === 4`, the responseText has finished downloading and you can begin to parse the response.

- You may also attach an `onload` listener, which will only be fired if the request is successful. If you use this, you should also use an `onerror` listener, which will fire if the request was unsuccessful and will allow you to handle this gracefully.


[here]: http://www.bambielli.com/XmlHttpRequest-Example/
[wrote]: https://github.com/bambielli/XmlHttpRequest-Example/blob/master/assets/script.js
