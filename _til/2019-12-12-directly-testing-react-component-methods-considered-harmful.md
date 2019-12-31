---
title:  "Why Directly Testing React Component Methods is Considered Harmful"
date:   2019-12-12 09:00:00
category: til
tags: [React, component, methods, considered harmful, Facebook, airBnB, frontend, UI, testing, jest, enzyme, white box, black box, white-box, black-box, wrapper.instance, simulation, jest.spyOn]
---

TIL that directly testing React component methods is actually an antipattern.

This post is in response to a post I wrote [about a year ago][year]{:target="_blank"} that demonstrated a way of testing the private methods of a react component by accessing a component's instance from its enzyme wrapper using `wrapper.instance()`.

Since that post, I've come to realize that this is a form of [white box testing][white-box]{:target="_blank"}. 

White box testing involves peeking into your component's internals and accessing private implementation details during tests. The name "white box" comes from lifting up the shroud of the public interface that your component presents, making the internals `visible` to the tester.

Compare this to [black box testing][black-box]{:target="_blank"}: instead of accessing a component's private methods or implementation details, the tester just tests what a component makes publicly available. For most components this just involves testing the expected behavior in response to interactions like clicks or hovers. The name "black box" comes from leaving the shroud of your component's public interface in place, making the internals `invisible` to the tester.

## Issues with White Box Testing

Testing a component method directly in a white box manner typically leads to overspecification of your component. **You wind up testing component impelementation details (the "how"), instead of component behavior (the "what").** This can result in brittle designs and components that become difficult to change. 

Furthermore, if you feel the need call a component method directly in a test, this is a [code smell][smell]{:target="_blank"} that indicates your component is likely too complex and should be simplified. 

## Example Component

Here is the same `home` component we tested in my previous post. It contains a button, a piece of counter state, and a div to display the current count.

{% highlight javascript %}

export default class Home extends Component {
  constructor(props) {
    super(props)
    this.state = {
      counter: 0,
    }
  }
  incrementCounter = (two) => {
    let counter;
    counter = two ? this.state.counter + 2 : this.state.counter + 1;
    this.setState({ counter })
  }

  render() {
    const { two } = this.props;
    return (
      <div>
        <div id="counter">{this.state.counter}</div>
        <button onClick={() => this.incrementCounter(two)} >Increment</button>
      </div>
    )
  }
}

{% endhighlight %}

Let's cover a white box and a black box approach to testing this component.

## White Box Testing Approach

In a white box approach, I access the private `incrementCounter` method directly using [`wrapper.instance()`][instance]{:target="_blank"}. The following is the same test suite that I used in my previous post:

{% highlight javascript %}

describe('directly invoking the "incrementCounter" method from component instance', () => {
  it('should update the count by 1 when invoked by default', () => {
    const wrapper = shallow(<Home />);
    const instance = wrapper.instance();
    expect(wrapper.state('counter')).toBe(0);
    instance.incrementCounter();
    expect(wrapper.state('counter')).toBe(1);
  });
  it('should add two to the counter when the "two" value is true', () => {
    const wrapper = shallow(<Home />);
    const instance = wrapper.instance();
    expect(wrapper.state('counter')).toBe(0);
    instance.incrementCounter(true);
    expect(wrapper.state('counter')).toBe(2);
  });
});

{% endhighlight %}

Notice that not only am I accessing `incrementCounter` directly, but I'm also accessing the component's internal state  to verify that the count updates. **Accessing state directly also constitutes white box testing**. What if I wanted to make a change to an implementation detail of my component, like changing the name of the property in state from `counter` to `count`? The tests above would break in response to this simple name change while the component would still behave as expected... 

**This is a symptom of the overspecification that white box testing can cause.** It is not a requirement that state has a property named `counter` but by including this in a test I have inadvertently made it a requirement. 

The same issue would present if the `incrementCounter` method's name changed. Tests would fail, even though component behavior is correct.

## Black Box Testing Approach

Compare this to the following black box test for the same component. Notice that instead of accessing `incrementCounter` directly, I'm using enzyme to simulate a click event on the button.

{% highlight javascript %}
describe('indirectly testing "incrementCounter" through click simulation', () => {
  it('should update the displayed count by 1 when invoked by default', () => {
    const wrapper = shallow(<Home />);
    expect(wrapper.find('#counter').text).toBe('0');
    wrapper.find('button').simulate('click');
    expect(wrapper.find('#counter').text).toBe('1');
  });
  it('should update the displayed count by two when the "two" prop is true', () => {
    const wrapper = shallow(<Home two />);
    expect(wrapper.find('#counter').text).toBe('0');
    wrapper.find('button').simulate('click');
    expect(wrapper.find('#counter').text).toBe('2');
  });
});
{% endhighlight %}

The public interface of this component is a clickable button that updates a count and displays that count. By simulating click events on the button, and verifying the count updates in response, I'm exercising the component in a way that is similar to how it would be used in the real world. The lesson here is to **verify your public interface, and leave all other implementation details alone**.

## For the Astute Reader

An astute reader will notice a an additional difference between the black box and white box test suites above.

Instead of accessing state directly via `wrapper.state` (like in the white box suite), I'm verifying that the count updated in response to a button click by selecting the displayed value (see `wrapper.find('#counter')`).

This eliminates yet another inadvertant coupling to an implementation detail of the component. The tester shouldn't care that when the button is clicked, a piece of state named `counter` internal to the component is updated. The user sure doesn't care about that. **The tester should just care that when the button is clicked, the component renders an updated click count in the appropriate place**. It's as simple as that!

I think it's actually good practice to ask yourself **would a user care about this** when writing a test. If so, it's likely part of the public interface and should be tested. If not, leave the test out!

## Conclusion: White Box Testing Considered Harmful

In summary, be aware of when you are lifting up the shroud of your component's public interface during tests. This leads to white box testing that adds unnecessary specificity to your application, making your code more difficult to change. 

Instead, just focus on testing the public interface of your component by simulating the actual interactions a user might have with it. Then, verify that your component behaves correctly in response to those interactions from the perspective of the end user.

[year]: /2018-03-04-directly-test-react-component-methods.md
[white-box]: https://en.wikipedia.org/wiki/White-box_testing
[black-box]: https://en.wikipedia.org/wiki/Black-box_testing
[smell]: https://en.wikipedia.org/wiki/Code_smell
[instance]: http://airbnb.io/enzyme/docs/api/ReactWrapper/instance.html