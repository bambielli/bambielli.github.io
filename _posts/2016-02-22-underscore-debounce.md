---
layout: post
title:  "Debounce Strikes Back: This Time with Underscore.js"
date:   2016-02-22 18:58:00
category: til
tags: [angular, javascript, underscore-js]
---

TIL that `underscore.js` has an awesome debounce wrapper that grants debouncing capabilities to any custom function.

[A few TILs back][debounce-post]{:target="_blank"} I wrote about angular's `ng-model-options` directive. __I needed a way to debounce the auto-calling of one of our API endpoints in response to text a user was entering into a text field.__ `ng-model-options` offered a way to do this with its debounce key, but it is only available in angular versions >= 1.3 and we are still stuck in 1.2.27.

Before we took the time to fix incompatibilities between angular 1.2.27 and 1.3, I stumbled upon another debouncing option offered by `underscore.js` which fit my requirements nicely.

The [`_.debounce`][underscore-debounce]{:target="_blank"} method accepts a function to be wrapped and a time in ms, and __returns a wrapped function with the specified debounce duration.__ This wrapped function returned from debounce is substitutable wherever the original unwrapped function would have gone in your code. Any calls to the wrapped function will reset the debounce timer, and the inner fn will only be called one time when the debounce timer reaches 0.

__Example:__

{% highlight javascript %}

$scope.apiCall = function() {
	console.log("I've been called!");
};

var wrappedFunction = _.debounce($scope.apiCall, 1000);

$scope.$watch('name', function(name) {
	wrappedFunction();
});

{% endhighlight %}

In the above example, whenever `$scope.name` changes the watcher will fire and will call the wrappedFunction returned from `_.debounce`. The wrapped function will wait for additional calls for 1 second. If no additional calls to the wrappedFunction are made, that means the user has (probably) finished typing input, and debounce will fire off our API call.

Debouncing was particularly important in this scenario, for performance reasons: since the watcher fires immediately upon detecting change in `$scope.name`, *it would have called our API one time for each letter input or deleted.* This would have led to an unacceptable number of pointless requests, and unnecessary load on the API server.

`underscore.js` saves the day again! I guess we'll upgrade angular sometime down the road...

[debounce-post]: https://www.bambielli.com/til/2016/02/18/angular-model-options-debounce.html
[underscore-debounce]: http://underscorejs.org/#debounce
