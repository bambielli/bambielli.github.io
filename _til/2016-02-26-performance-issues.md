---
title:  "Hunting Down Performance Issues"
date:   2016-02-26 12:38:00
category: til
tags: [angular, angular-animate, performance]
---

TIL what the [angular-animate][angular-animate]{:target="_blank"} library does, after noticing it was contributing to *over 60% of the scripting time necessary to scroll through our grid!*

angular-animate is a cool library provided by the angular team, that facilitates animations on the core angular ng-* directives. It works by applying a base set of classes to any element that contains an ng-*. These classes allow developers to attach custom CSS animations to events that map to the classes applied by angular-animate.

__Example:__

angular-animate applies three classes to the `ng-repeat` directive: `enter`, `leave`, and `move`. These are used to trigger animations when items are added (enter) removed (leave) or shuffled (move) in a collection. As a developer, I could create a class for my ng-repeat element, that adds or removes additional properties based on if one of the angular-animate classes is currently present.

See a list of the directives supported by angular-animate, and what classes are applied by default [here][here]{:target="_blank"}.

In the grid I've been working on over the past week, the addition and removal of these base classes on elements that entered the grid became a serious performance concern: __We have `ng-repeat` and `ng-if` directives within cells of each grid row__ which led to a huge number of class additions and removals by angular-animate.

By disabling angular-animate for directives in the grid, __I reduced scripting time during scroll events by 60%.__ Instead of stopping there, I checked to see if we were actually using angular-animate at all throughout our core app...we weren't. I removed the service from the entire module which will now save performance time across our core app.


[angular-animate]: https://docs.angularjs.org/api/ngAnimate
[here]: https://docs.angularjs.org/api/ngAnimate#directive-support
