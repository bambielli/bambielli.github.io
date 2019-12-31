---
title:  "Directly Testing React Component Methods Considered Harmful"
date:   2019-12-12 09:00:00
category: til
tags: [React, Component, Methods, Considered Harmful, Facebook, AirBnB, Frontend, UI, Testing, Jest, Enzyme]
---

TIL that directly testing React component methods is actually an antipattern.

This post is in response to a post I wrote [about a year ago][year] that highlighted a way of testing private methods of a react component by accessing the wrapped component instance from its enzyme wrapper using `wrapper.instance()`.

Since that post, I've come to realize that this is a form of [white box testing][white-box]. By testing a private component method directly, you are likely overspecifying your component. You wind up testing the "how" for a component impelementation, instead of the "what". This can lead to brittle designs and components that become difficult to change.

{insert example component, show private method, show testing it using wrapper.instance}

Compare this to [black box testing][black-box].

{describe why this is preferable}
{insert example component, show simulating event}

{conclusion}

[year]: /2018-03-04-directly-test-react-component-methods.md
[white-box]: https://en.wikipedia.org/wiki/White-box_testing
[black-box]: https://en.wikipedia.org/wiki/Black-box_testing
