---
layout: post
title:  "Angular-Animate...Slowing Us Down!"
date:   2016-02-26 12:38:00
category: til
tags: [angular, angular-animate, performance]
---

TIL what the [angular-animate][angular-animate]{:target="_blank"} library does, after noticing it was contributing to over 60% of the scripting time necessary to scroll through our grid!

angular-animate is a cool library provided by the angular team, to facilitate animations on the core angular ng-* directives. After installing it through your package manager, inject it as a service (ngAnimate) in the base of the modules that you want to use it.

For all of the core angular directives, angular-animate applies a set of classes to each that facilitate class-based CSS animations. `ng-repeat` for example gets 3 classes applied by angular-animate: enter, leave, move. These are used to trigger animations when items are added (enter) removed (leave) or shuffled (move) in a collection. See a list of the directives supported by angular-animate, and what classes are applied by default [here][here]{:target="_blank"}.

In the grid I've been working on over the past week, the application and removal of these classes on elements that entered the grid became a serious performance concern. __We have ng-repeat and ng-if directives within cells of each grid row__ leading to a huge number of class additions and removals from angular-animate.

By disabling the application of the base angular-animate classes for the grid, I reduced scripting time during scroll events by 60%. I was just going to disable angular-animate entirely for the grid, but realized that we don't actually use the library anymore through the app, so I removed it entirely.

angular-animate is a cool library in concept, but this specific application where we had fast moving / dynamic elements scrolling through a grid seemed to be an inappropriate use.

[angular-animate]: https://docs.angularjs.org/api/ngAnimate
[here]: https://docs.angularjs.org/api/ngAnimate#directive-support
