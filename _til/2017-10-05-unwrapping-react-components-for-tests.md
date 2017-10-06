---
title:  "Unwrapping React Components for Testing Purposes"
date:   2017-10-05 20:37:00
category: til
tags: [javascript, testing, test, jest, react, node, mobx, mobx-react, HOC, enzyme, react-i18next]
---

TIL how to unwrap react components that are decorated by higher order component wrappers like `mobx`, to get at the underlying component implementation during snapshot tests.

## The problem

A typical container component for a view in a react application is connected to stores. This allows the component to trigger store actions that have side effects, like API calls, and to read data that has been cached in the store. For the purposes of this post, I'm going to be talking about mobx as the store provider, instead of redux.

Stores are injected to a component using mobx-react's [`inject`][inject]{:target="_blank"} decorator. From the docs: *inject is a higher order component (HOC) that takes a list of strings (stores) and makes those stores available to wrapped components.*

The issue during testing comes when you want to assert on the shallow-rendered output of a component that is connected to stores (wrapped in inject). Enzyme's [`shallow render`][shallow]{:target="_blank"} method, by design, stops component rendering **one level deep.** This is useful for limiting the scope of our component rendering to the component under test, preventing bugs in lower level components from causing errors in higher level component tests.

Remember the golden rule: [**write tests that are useful.**][useful] Useful tests help a developer fix a bug quickly, by targeting their focus to the module that caused the bug. Shallow rendering aids in this targeting.

Since our container components are wrapped in these HOCs that connect them to the mobx stores, when we shallow render a connected component the **rendering stops at the higher order component itself.** In other words: shallow rendering prevents any rendered output from our component under test from being added to our enzyme wrapper, since it is wrapped in a HOC!

## Solution: "Unwrap" the higher order component

We don't want to assert on the HOC in our tests, we want to assert on the component's rendered output itself. To do this, mobx-react provides a method to access the content of the wrapped component. The API is literally called `wrappedComponent`. See an example below:

{% highlight javascript %}

import { shallow } from 'enzyme';
import ContainerComponent from './ContainerComponent';

const props = {
    data: [...]
}

describe('ContainerComponent tests', () => {
    it('should render when data is provided', () => {
        const wrapper = shallow(<ContainerComponent.wrappedComponent {...props} />);
        expect(wrapper).toMatchSnapshot();
    });
});

{% endhighlight %}

The code above makes a snapshot test of the rendered output of the ContainerComponent in response to data being present in props. Note the use of `wrappedComponent` in the construction of a new component instance: this is the critical piece to be able to peel away the mobx HOC wrapper around the connected container component, and get at the component under test.

## Bonus TIL

We also use the package `react-i18next` for localization. This package provides yet another higher order component called `translate`, that provides a translate function via props to the wrapped component for localization purposes.

In the case where a component is both wrapped by the `translate` HOC and the `inject` HOC, **we found we needed to unwrap both of these layers during unit tests to get at the inner component implementation.**

Fortunately, [`react-i18next`][i18]{:target="_blank"} provides a similar API to `mobx-react` for getting at the wrapped component instance. To unwrap the translate HOC, use the method `WrappedComponent`. Note the capital `W` used to unwrap a component from `react-i18next`, and the lower case `w` for unwrapping a mobx connected component...

{% highlight javascript %}

import { shallow } from 'enzyme';
import ContainerComponent from './ContainerComponent';

const props = {
    data: [...]
}

describe('ContainerComponent tests', () => {
    it('should render when data is provided', () => {
        // note the use of WrappedComponent and wrappedComponent
        const wrapper = shallow(<ContainerComponent.WrappedComponent.wrappedComponent {...props} />);
        expect(wrapper).toMatchSnapshot();
    });
});

{% endhighlight %}

This syntax was quite confusing, and it took us a while to realize what was going on when we were trying to correctly unwrap our components during tests.

[inject]: https://github.com/mobxjs/mobx-react#provider-and-inject
[shallow]: http://airbnb.io/enzyme/docs/api/shallow.html
[useful]: /posts/2017-10-02-useful-tests/
[i18]: https://github.com/i18next/react-i18next
