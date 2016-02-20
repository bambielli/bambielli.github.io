---
layout: post
title:  "ng-bind vs interpolation"
date:   2016-02-19 21:49:00
category: til
tags: [angular]
---

TIL that instead of interpolating a data binding in a template (via double curly brace syntax `{{}}`), you can add the [`ng-bind`][ng-bind]{:target="_blank"} directive to a template tag.

For the most part the behaviors of using ng-bind and interpolation are identical, with a two differences:

1) If using interpolation, and the template renders before the necessary data has been loaded / bound to scope, the viewer will briefly see the double curly braces rendered in the browser until the required data is bound. Since ng-bind is an element attribute, it does not have this issue. A way to get around the interpolation concern is to use ng-cloak to hide portions of a template until it and its data bindings have finished loading, but the ng-bind solution is less verbose.

2) __Interpolation is slower than using ng-bind!__ The ng-bind directive places a watcher on the property to which it is bound: this watcher will only fire when that property changes. Interpolation, however, is dirty-checked and refreshed on **every digest cycle** regardless of if it changed or not.


__Example:__

{% highlight html %}
	<input ng-model="user.name"></input>
	<!-- using interpolation -->
	<div>
		Hello, {{user.name}}
	</div>
	<!-- using ng-bind -->
	<div>
		Hello, <span ng-bind="user.name"></span>
	</div>
{% endhighlight %}

I'll be using ng-bind more often, for sure.

[ng-bind]: https://docs.angularjs.org/api/ng/directive/ngBind
