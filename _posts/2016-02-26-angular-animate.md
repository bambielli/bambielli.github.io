---
layout: post
title:  "Hunting Down Performance Issues: 'angular-animate'"
date:   2016-02-26 12:38:00
category: til
tags: [angular, angular-animate, performance]
---

TIL what the [angular-animate][angular-animate]{:target="_blank"} library does, after noticing it was contributing to over 60% of the scripting time necessary to scroll through our grid!

angular-animate is a cool library provided by the angular team, to facilitate animations on the core angular ng-* directives. It works by applying a base set of classes to any element that contains an ng-* directive. These classes facilitate CSS based animations by allowing developers to define custom animations for different events using the angular-animate base class names.

__Example:__

angular-animate applies three classes to the `ng-repeat` directive: `enter`, `leave`, and `move`. These are used to trigger animations when items are added (enter) removed (leave) or shuffled (move) in a collection.

See a list of the directives supported by angular-animate, and what classes are applied by default [here][here]{:target="_blank"}.

In the grid I've been working on over the past week, the addition and removal of these base classes on elements that entered the grid became a serious performance concern. __We have `ng-repeat` and `ng-if` directives within cells of each grid row__ which led to a huge number of class additions and removals from angular-animate.

By disabling the application of the base angular-animate classes for directives in the grid, __I reduced scripting time during scroll events by 60%.__ I was just going to disable angular-animate for the grid, but realized that we actually don't use the library anymore in the module containing the grid, so I removed it entirely.


[angular-animate]: https://docs.angularjs.org/api/ngAnimate
[here]: https://docs.angularjs.org/api/ngAnimate#directive-support
