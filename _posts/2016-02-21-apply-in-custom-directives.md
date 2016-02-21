---
layout: post
title:  "Why $scope.apply() is Necessary in Custom Directives"
date:   2016-02-21 11:47:00
category: til
tags: [angular]
---

TIL why you need to call `$scope.apply()` when you change the value of `$scope` properties inside of custom angular directives.

[`$scope.apply()`][apply]{:target="_blank"} is a way to inform angular that a data model has changed in the current context. __Calling `$scope.apply()` implicitly calls `$rootScope.$digest()`,__ so every watcher that is registered to `$rootScope` on down to the currently existing child scopes will be dirty-checked to detect models changes.

`$scope.apply()` is called automatically by all of angular's built in directives and services that have the ability to update data models (e.g `ng-bind`, `ng-click`, `$http`, `$timeout`). We need to call `$scope.apply()` manually __if a data model has the potential to change outside of an angular context:__ i.e. when we are changing `$scope` properites inside of custom directives.

__Example:__

We use the [jquery-ui][jquery-ui]{:target="_blank"} library at work to create some custom [slider][slider]{:target="_blank"} elements. We implement the interface for the slider inside of a custom directive. The directive is passed a range of values (the upper and lower bounds for the slider) which are bound to a `$scope` property in the parent `$scope` called `$scope.slider_values`. There is a watcher registered on `$scope.slider_values` that executes `$http` requests to fetch new data from the backend when slider_values change.

Our custom slider directive implements a callback bound to the slider `onChange` event that fires when the upper or lower bounds for the slider change (event fires on mouseUp). Since our directive binds its `isolated_scope.slider_values` property to the `$scope.slider_values` poperty in the parent scope (via the '=' assigner), I initially thought that just setting the new upper and lower bounds to `isolated_scope.slider_values` inside of the onChange callback would be detected on the parent scope.

__This was not the case:__ I needed to wrap the assignment of my new slider range values to `isolated_scope.slider_values` in an `isolated_scope.apply()` to inform angular that a value had been changed on the parent scope. My custom directive does not automatically call `apply()` like angular's built in directives do, so calling it manually is necessary in this case. Remember that calling `apply()` triggers an implicit call to `$rootScope.$digest()`, so even though I called `apply()` on my isolated_scope, it still triggered the appropriate watchers bound to the parent `$scope`.

__Bonus TIL:__

One last thing to note about `apply()`...

`apply()` takes an optional function argument that can be used to wrap code that makes the model changes to be applied. It is possible to call `$scope.apply()` without the optional function, but if your code throws an error it will be thrown outside of angular's exception handling mechanisms. Wrapping model-changing code inside of the `$scope.apply()` function argument puts it within an angular context, exposing it to these exception handling mechanisms, so it is always best practice to do so.

__Example:__

{% highlight javascript %}

//it is possible to apply the updates of model-altering code using $scope.apply() without the optional function wrapper.
isolated_scope.name = 'Brian';
isolated_scope.apply(); // this will apply the updated name to scope.name


// It is considered best practice, though, to wrap code that needs to be applied inside of the optional function,
// to take advantage of angulars exception handling mechanisms.
isolated_scope.apply(function(){
	isolated_scope.name = 'Brian';
});

{% endhighlight %}

I'm starting to get a better handle on the angular `$digest()` cycle, and implementing this directive really helped solidify my understanding about what is going on in the background of angular events.

[apply]: https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$apply
[jquery-ui]: https://jqueryui.com/
[slider]: https://jqueryui.com/slider/
