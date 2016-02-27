---
layout: post
title:  "One Time Binding"
date:   2016-02-25 11:50:00
category: til
tags: [angular, performance]
---

TIL that angular 1.3 and onwards has a `one-time-bind` option to pass to interpolated expressions in templates, which tells angular that the data is not expected to change.

The one-time-bind option tells angular not to add a watcher to the interpolated property, *effectively disabling 2-way binding for the property.* If you know a `$scope` property is not expected to change during the lifetime of the view, why waste the extra time during the `$digest()` cycle to dirty-check the property?

__Example:__

{% highlight html %}
<!-- Normal 2-way binding (creates a watcher on $scope.name) -->
<span>Hello, {%raw%} {{ name }} {%endraw%}</span>

<!-- one-time binding (no watcher created) -->
<span>Hello, {%raw%} {{ ::name }} {%endraw%}</span>

{% endhighlight %}

The double semicolon `::` lets angular know that it should do a one time binding of the scope property to the template. If `$scope.name` changes in the second situation above, the change will not be reflected in the template.

Another reason to upgrade to angular 1.3!

[ng-bind]: https://docs.angularjs.org/api/ng/directive/ngBind
