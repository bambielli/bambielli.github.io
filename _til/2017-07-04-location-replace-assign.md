---
title:  "location.replace() and location.assign()"
date:   2017-07-04 8:56:00
category: til
tags: [javascript, browser, client, react-router]
---

TIL how to use `location.replace()` and `location.assign()` to change URLs programmatically in the browser.

[location.assign()][assign]{:target="_blank"} causes the browser to navigate to the URL provided. The provided URL **is added to the browser's history**: a user can click the back button and navigate back to the page they were on before `assign()` was called.

[location.replace()][replace]{:target="_blank"} behaves almost identically to `location.assign()`, however instead of adding the provided URL to the browser's history, `replace()` **replaces** the current URL in history with the one provided. This effectively erases the current URL from the browser's history: **the user won't be able to click the back button to navigate back to the page they were on before `replace()` was called.**

## A small, but Important, Detail

The argument to either assign() or replace() can be a **full URL**, or a **relative URI**.

If a relative URI is passed, the browser will append the relative URI to the end of the base path for the current domain. A **leading slash** `/` in the provided URI will result in the entire pathname being replaced with the provided URI. If no leading `/` is provided, only the last portion of the pathname will be replaced! This caveat confused me for quite some time when I was trying to programmatically manipulate history in the context of a react-router application.

Check out the following examples to see the behavior:

{% highlight javascript %}
// Full URL
// current URL: http://localhost:3000/users/1
location.assign('https://developer.mozilla.org/en-US/docs/Web/API/Location/assign').
// new URL: https://developer.mozilla.org/en-US/docs/Web/API/Location/assign

// Relative URI with leading slash
// current URL: http://localhost:3000/users/1
location.assign('/accounts/1').
// new URL: http://localhost:3000/accounts/1

// Relative URI without leading slash
// current URL: http://localhost:3000/users/1
location.assign('2').
// new URL: http://localhost:3000/users/2
{% endhighlight %}

## Manipulating History with react-router

When using react-router's history API, `history.push()` acts like `location.assign()` and `history.replace()` acts like, you guessed it, `location.replace()`.

Using these methods with hashRouter can be somewhat confusing to think about... hashRouter keeps track of its 'pathname' after the hash in the URL. This means that the value stored in `history.location.pathname` is equivalent to the value stored in `window.location.hash`! `history.location.pathname` should therefore **always be in sync** with `window.location.hash`.

I ran in to a weird bug regarding this recently that I think I will write about in a separate post. Find the related [Stack Overflow post here.][so]{:target="_blank"}

[assign]: https://developer.mozilla.org/en-US/docs/Web/API/Location/assign
[replace]: https://developer.mozilla.org/en-US/docs/Web/API/Location/replace
[so]: https://stackoverflow.com/questions/44896414/keycloak-redirect-fragment-conflicts-with-react-router-hashrouter


