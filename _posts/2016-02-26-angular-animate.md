---
layout: post
title:  "Angular-Animate...Slowing Us Down!"
date:   2016-02-26 12:38:00
category: til
tags: [angular, angular-animate, performance]
---

TIL what the [angular-animate][angular-animate]{:target="_blank"} library does, after noticing it was contributing to over 60% of the scripting time necessary to scroll through our grid!

angular-animate is a cool library provided by the angular team, to facilitate animations on the core angular ng-* directives. After installing it through your package manager, inject it as a service (ngAnimate) in the base of the modules that you want to use it.

angular-animate applies a base set of classes to elements that contain any of the core angular directives. These classes facilitate CSS based animations by allowing you to define your own animations for different events using the angular-animate base class names.

`ng-repeat` for example gets 3 classes applied by angular-animate: `enter`, `leave`, and `move`. These are used to trigger animations when items are added (enter) removed (leave) or shuffled (move) in a collection. See a list of the directives supported by angular-animate, and what classes are applied by default [here][here]{:target="_blank"}.

In the grid I've been working on over the past week, the addition and removal of these base classes on elements that entered the grid became a serious performance concern. __We have ng-repeat and ng-if directives within cells of each grid row__ which led to a huge number of class additions and removals from angular-animate.

By disabling the application of the base angular-animate classes for directives in the grid, __I reduced scripting time during scroll events by 60%.__ I was just going to disable angular-animate for the grid, but realized that we actually don't use the library anymore in the module containing the grid, so I removed it entirely.


[angular-animate]: https://docs.angularjs.org/api/ngAnimate
[here]: https://docs.angularjs.org/api/ngAnimate#directive-support
