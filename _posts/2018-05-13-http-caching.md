---
title:  "Tech Talk: HTTP Caching"
date:   2018-05-13 08:30:00
category: post
tags: [http, caching, http caching, etag, last-modified, if not modified, if no match, cache-control, no-cache, max-age, validate token, expires, pragma, public, private, cache, browser cache, hapijs, webpack, chunkhash, commonchunks, webpack manifest]
---

This week I prepared a presentation for Uptake's front end community of practice on HTTP Caching.

The [Developer Portal][devportal] team at Uptake recently overhauled how we performed HTTP caching of static assets for the site.

## etags

We began with an `etag` based validation token strategy for static assets, which required that we validate the freshness of cached static files via HTTP requests to the server on each subsequent page refresh.

**This was a waste!** Our static files rarely changed (particularly our vendor code and images) so performing these freshness checks just resulted in unnecessary chattiness with the webserver.

## chunkhashing and cache-contol

We ended up moving to a `webpack chunkhash` + `cache-control: max-age` strategy, which allowed us to cache our static assets in the browser **indefinitely.**

A [chunkhash][chunkhash] acts in a similar way as an etag. It is generated based off of the content of your static file: in other words, **a chunkhash will only change if the content of your file changes**.

Adding a chunkhash to the names of your static assets allows you to cache indefinitely, as the cache will be busted with a new chunkhash the next time the contents change and a bundle with a brand new name is requested. Until that time, it's ok to keep using the cached version.

We wound up saving an average of around **100ms** per page load, and about **2Kb** of data over the the etag strategy. Both strategies were very easy to configure. There really isn't any excuse NOT to be caching static assets!

Find the slides that I presented below. They were heavily inspired from [this blog post][goog] on HTTP caching by Ilya Grigorik.

[If you are on mobile, click here for a PDF of the presentation.][here]

<embed src="/assets/pdf/Http-Caching.pdf" />


[devportal]: https://developer.uptake.com
[chunkhash]: https://webpack.js.org/guides/caching/#output-filenames
[goog]: https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching
[here]: /assets/pdf/Http-Caching.pdf

