---
title:  "URLSearchParams Object"
date:   2017-08-11 13:22:00
category: til
tags: [front-end, javascript, URLSearchParams]
---

TIL about the URLSearchParams Object, which is useful for parsing URL querystrings in to maps of keys and values.

[`URLSearchParams`][params]{:target="_blank"} supplies a constructor that parses a string formatted as a URL querystring in to a key-value map.

The object also exposes a few useful methods for manipulating the querystring, checking for the presence of certain keys, and converting back from the map to a URL safe string.

{% highlight javascript %}

const queryStringMap = new URLSearchParams('?type=blue&action=create');

queryStringMap.has('type') // returns true
queryStringMap.get('type') // returns 'blue'
queryStringMap.set('type', 'green') // sets type key to value 'green'
queryStringMap.append('name', 'brian') // adds key 'name' and value 'brian' to map
queryStringMap.toString() // returns 'type=green&action=create&name=brian'

{% endhighlight %}

I can't tell you the number of times I've written querystring parsing and encoding logic in projects... this object does it all for you!

One thing to note: the `toString()` method **does not prepend a question mark in front of the encoded key-value pairs**, so you'll need to provide that yourself.

According to MDN, basic support for `URLSearchParams` has been available since Chrome 49, Firefox 29, Safari 10.1, and is not supported by IE. This means if you want to use the object in your client-side code, **you should be prepared to polyfill for legacy browsers + IE.**

The object has also been available in [node environments][node]{:target="_blank"} since v7.5.0, and npm packages exist to [polyfill the behavior][poly]{:target=""} for older node environments if you're stuck on a legacy version.

[params]: https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams
[node]: https://nodejs.org/api/url.html#url_class_urlsearchparams
[poly]: https://www.npmjs.com/package/url-search-params