---
title:  "Fetch API: Options"
date:   2017-07-03 20:54:00
category: til
tags: [javascript, front-end]
---

TIL how to pass headers along with requests initiated by the `fetch` API.

### What is Fetch

The [fetch API][fetch]{:target="_blank"} provides a simple way to make network requests from clients. While it exists natively on the window object in all but our most favorite of modern browser (\*cough\* IE \*cough\*) there are polyfills to fill the behavior if it is missing. There is also a library called [isomorphic-fetch][iso]{:target="\_blank"} that allows you to use the same fetch API in your server-side javascript applications.

The gist of `fetch` is that it accepts one mandatory argument, the *path* that it should initiate a request to, but it also accepts a companion object with different configuration options (like `method` and `body`).

A difference to note between fetch and jQuery.ajax(), is that **fetch will not reject the promise if the response from the request results in 404 or 500.** Instead it will resolve, with `response.ok === false`. Be wary before blindly parsing the JSON of all resolved requests, since 404 and 500 will not have any response body to parse **which will result in a runtime error if parsed**.

Here is an example of what initiating a POST request to create a new user might look like using fetch:

{% highlight javascript %}
fetch('/api/users', {
    method: 'POST',
    body: {
        name: 'brian',
        profession: 'blogger',
    },
}).then((res)=>{
    if (res.ok) {
        console.log('user successfully created!')
    }
}).catch((err) => {
    console.log('there was an error', err.message)
});
{% endhighlight %}

We use the fetch API at work, and find the syntax simple to use and reason about. We've even jumped on the isomorphic-fetch bandwagon, so we're using fetch both client and server side!

### Adding Headers

I needed to pass a few headers along with a request I was making. I thought I could just put my headers at the top level of the fetch companion object like so:

{% highlight javascript %}
fetch('/api/users', {
    Authorization: 'nunya business'
})
{% endhighlight %}

This didn't work as I was expecting though... I didn't realize that there was actually a **top level key for headers** expected in the companion object! I also didn't realize that there was an entire [Headers class][headers]{:target="_blank"} that was expected to be passed as the value of that headers key!

The headers class is simple enough to use: it offers some getters and setters, but also allows you to pass in an object that contains a mapping from header name to header value in the constructor of a new headers object. See example below for construction of the headers object, and passing it as a top-level key in the fetch companion object for the POST request from before:

{% highlight javascript %}
const headers = new Headers({
    Authorization: "nunya business",
});

fetch('/api/users', {
    method: 'POST',
    headers: headers,
    body: {
        name: 'brian',
        profession: 'blogger',
    },
}).then((res)=>{
    if (res.ok) {
        console.log('user successfully created!')
    }
}).catch((err) => {
    console.log('there was an error', err.message)
});
{% endhighlight %}

Let me know if you found this helpful! Feel free to drop a comment.

[fetch]: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
[iso]: https://github.com/matthew-andrews/isomorphic-fetch
[headers]: https://developer.mozilla.org/en-US/docs/Web/API/Headers
