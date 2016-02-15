---
layout: post
title:  "Angular Services Maintain State Between Browser Refreshes"
date:   2016-02-12 15:47:00
category: til
tags: [angular, javascript]
---

TIL that angular services maintain internal state until the page is reloaded.

The underlying logic behind this is that services are singletons: regardless of how many times you inject the service throughout your app, your are injecting a reference to the same object.

__Example:__ Say I have an angular service with a private field, and public methods that access / mutate it.

{% highlight javascript %}
angular.module('myModule').service('myService', ['...', function (...) {
	var private_boolean = false;

	...

	this.get_private_bool() {
		return private_boolean;
	}

	this.update_private_bool() {
		private_boolean = !private_boolean;
	}


}]);
{% endhighlight %}

If I inject this service in to two different controllers, the state of private_boolean will be shared between the two.

> If controller1 calls myService.update_bool(), then controller2 accesses the private field via the accessor, that field will return with a value of true.

This state persists until the browser is refreshed, and the service is re-instantiated as static files are re-loaded in the browser.
