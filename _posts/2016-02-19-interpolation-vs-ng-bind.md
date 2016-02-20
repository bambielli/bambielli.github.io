---
layout: post
title:  "Angular Interpolation vs. ng-bind"
date:   2016-02-19 21:49:00
category: til
tags: [angular]
---

TIL that instead of interpolating a data binding in a template (via double curly brace syntax), you can accomplish the same thing by adding the [`ng-bind`][ng-bind]{:target="_blank"} directive to a template tag.

The behaviors between ng-bind and interpolation are very similar, with two main differences:

1) If using interpolation, and the template renders before the necessary data has been loaded / bound to scope, *the viewer will briefly see the double curly braces rendered in the browser until the required data is bound.* Since ng-bind is an element attribute, it does not suffer from this issue.

  - Note: A way to around the interpolation concern is to use ng-cloak to hide portions of a template until it and its data bindings have finished loading, but the ng-bind solution is less verbose.

2) __Interpolation is slower than using ng-bind!__ The ng-bind directive places a watcher on the property to which it is bound: this watcher will only fire when that property changes. Interpolated bindings, however, are dirty-checked and refreshed on **every digest cycle** regardless of if they changed or not. This can lead to unnecessary updates and can slow your app down.


__Example:__

{% highlight html %}
<input ng-model="user.name"></input>
<!-- using interpolation -->
<div>
	Hello, {%raw%}{{ user.name }}{% endraw %}
</div>
<!-- using ng-bind -->
<div>
	Hello, <span ng-bind="user.name"></span>
</div>
{% endhighlight %}

The performance benefit of using ng-bind is reason enough to favor it over interpolation. I'll definitely be using it more for my data-binding needs.

[ng-bind]: https://docs.angularjs.org/api/ng/directive/ngBind
