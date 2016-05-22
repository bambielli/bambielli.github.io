---
title:  "Angular ng-model-options - Debounce"
date:   2016-02-18 18:55:00
category: til
tags: [angular]
---

TIL about the [`ng-model-options`][model-options]{:target="_blank"} directive, which augments the behavior of an input element bound to a scope property via [`ng-model`][model]{:target="_blank"}.

I was looking for a way to put a short time-delay on a text input in our app. The delay would prevent the ng-model scope property bound to the text input from updating, until we were *roughly* sure that the user had finished typing. This delay will probably wind up being somewhere between 350 and 500ms, which is nearly imperceivable to the end user.

Enter the ng-model-options directive, particulary the __debounce__ option. Debounce does exactly what I describe above: it waits a number of milliseconds before updating the scope property to which the input is bound.

__Example:__
{% highlight html %}
	<input ng-model="user.name" ng-model-options="{ debounce: 1000 }"></input>
{% endhighlight %}

When the user enters a username to the input above, `$scope.user.name` will only update **if no change is detected in the input field for 1000ms**. This prevents any watchers / API calls that are reactive based on user input from needlessly firing when the user hasn't finished inputting their full query yet.

The only remaining issue...ng-model-options was added in angular 1.3 and we are still on 1.2.27 at work...

I think we're due for an upgrade!

[model-options]: https://docs.angularjs.org/api/ng/directive/ngModelOptions
[model]: https://docs.angularjs.org/api/ng/directive/ngModel
