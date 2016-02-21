---
layout: post
title:  "Angular Watchers - What's the Difference?"
date:   2016-02-20 21:49:00
category: til
tags: [angular]
---

TIL what the difference between angular's 3 watcher configurations are: `$watch`, `$watchCollection`, and `$watch (objectEquality)`.

__Angular watchers facilitate the 2 way data binding between views and controllers / services.__ Watchers bind listener functions to $scope properties (or expressions that evaluate to $scope properties). Bound `$scope` properties are dirty-checked during each `$digest()` cycle, and if a change in the property is detected the bound listener function is called.

There are a few different variations of watchers, each with different performance and purposes:

### [$watch][watch]{:target="_blank"}

This is the most basic form of watcher. It accepts an expression as a string (either a `$scope` property or expression that evaluates to a `$scope` property) and a listener function to be called when the expression changes. This type of watcher only detects changes to __shallow__ aspects of the bound `$scope` property (reference equality via `===` comparison). This means if the bound `$scope` property is an object, and a value in that object changes, the listener callback will not be called.

__Example:__
{% highlight javascript %}

$scope.name = 'Brian'; // Initialize scope

$scope.$watch('name', function(new_val, old_val, scope) {
	console.log(new_val);
});

$scope.$digest(); // Value of $scope.name after $digest() is 'Brian'

// The following assignments would trigger the listener above.
// Note: between each assignment, the $scope would need to be $digest-ed for the change to be detected.
$scope.name = []; // A new array or object (a new reference)
$scope.name = undefined; // Making it undefined or null (which changes the reference)
$scope.name = 'hi'; // New strings
$scope.name = 1; // New primitives

// It will also be triggered when a new object with exactly the same values is passed (since reference is different).
$scope.name = ['Jim', 'John']; // triggers the watcher
$scope.name = ['Jim', 'John']; // triggers the watcher AGAIN since we created a new object (reference changed).

{% endhighlight %}

### [$watchCollection][watch-collection]{:target="_blank"}

This registeres a watcher for an array or object that is bound to $scope. If any item within the collection changes, including the addition or removal of a new item, the listener is called. Equality is determined using `===` between cycles.

__Example:__
{% highlight javascript %}
$scope.names = ['Brian', 'Alex', 'Sean'] // Initialize scope

$scope.$watchCollection('names', function(new_val, old_val, scope) {
	console.log(new_val);
});

$scope.$digest(); // Value of $scope.names after $digest() is ['Brian', 'Alex', 'Sean']


// The following assignments would trigger the listener above.
// Note: between each assignment below, the $scope would need to be $digest-ed for the change to be detected.
$scope.names = [] // A new array with different values
$scope.names = undefined // Changing reference to undefined
$scope.names.push('Corey') // Adding a new item to the collection
$scope.names.splice(0,1) // Removing an item from the collection
$scope.names[0] = 'Chris' // Changing a value at an index of the collection

// Note the difference between the first $watch and $watchCollection:
$scope.names = ['Jim', 'John']; // triggers the watcher
$scope.names = ['Jim', 'John']; // DOES NOT trigger the watcher again, even though it is a new object, since values are the same.

{% endhighlight %}

### [$watchGroup][watch-group]{:target="_blank"}

The distinction between this watcher and `$watchCollection`, is that `$watchGroup` takes a **group of expressions** to watch instead of just one collection object. This can be useful if you need to bind a group of `$scope` properties to the same listener callback (instead of needing to write different watchers for them all). `$watchGroup` is only available in agular 1.3 onwards, so we don't use it yet in our application (still on angular 1.2.27) and `$watchCollection` can be configured to perform similarly to `$watchGroup`.

### $watch (objectEquality)

`$watch` can take a third boolean argument, which tells the watcher to use `angular.equals` to check for deep objectEquality of the bound item. This causes the watcher to fire even when a sub-key or value of an object inside of the watched `$scope` property changes.

__Example:__
{% highlight javascript %}

$scope.full_name = [{'first_name':'Brian', 'last_name': 'Ambielli'}]; // Initialize scope

$scope.$watch('names', function(new_val, old_val, scope) {
	console.log(new_val);
}, true); // objectEquality boolean.

$scope.$digest() // Value of $scope.full_name after $digest() is [{'first_name':'Brian', 'last_name': 'Ambielli'}]

// This listener will be called for all situations outlined in the previous examples, including the following:
$scope.full_name[0].first_name = 'Lauren'; // changing a property of an object in the collection

// This watcher will NOT trigger its listener if a new object is assigned that is exactly the same
// as the original scope property, since angular.equals will return 'true' in this situation.
$scope.full_name = [{'first_name':'Michael', 'last_name': 'Ambielli'}] // Triggers the listener
$scope.full_name = [{'first_name':'Michael', 'last_name': 'Ambielli'}]; // WILL NOT trigger the listener

{% endhighlight %}

Because `$watch (objectEquality)` uses `angular.equals` for deep object comparison, it is the __slowest performing__ of the watchers (deep comparison takes longer than shallow). For this reason, if you just need to watch a list of items and you don't care about deep value changes in the list, `$watchCollection` is a better choice. Of course, if you just need to watch a single value (and shallow comparison will suffice) `$watch` without objectEquality is the best option.

__Bonus TIL:__ I also learned that registering a watcher on scope returns a deregistration function that can be used to remove the watcher from the digest cycle if it is no longer needed.

__Example:__

{% highlight javascript %}

var deregister = $scope.$watch('name', function(new_val, old_val, scope) {
	alert('watcher triggered!');
	deregister();
});

{% endhighlight %}

The next time `$scope.name` changes, the watcher will not be registered so the alert will not occur. *This is a good way to tidy up after yourself and reduce the length of the digest cycle,* if watchers only need to exist temporarily or under certain conditions. `$watchCollection` also returns a deregister function, so it can be used in the same way.


[watch]: https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$watch
[watch-collection]: https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$watchCollection
[watch-group]: https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$watchGroup
