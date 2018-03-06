---
title:  "How to Directly Test React Component Methods with Enzyme"
date:   2018-03-04 09:00:00
category: til
tags: [React, component, method, enzyme, airBnB, Facebook, testing, jest, instance, wrapper, wrapper.instance, simulation, jest.spyOn]
---

TIL how to directly test a React component method using enzyme `wrapper.instance()`

See here for a repo with a working version of the sample code covered in this post: [https://github.com/bambielli/testing-react-component-methods][github]{:target="_blank"}

A common pattern when testing React component methods using the AirBnB enzyme library, is to figure out what event triggers the method through normal usage of the component and `simulate` that event to indirectly trigger it.

While this is a valuable test to ensure your component behaves correctly in response to events, it can become tedious and difficult to configure a component in just the right way to fully exercise a complex method indirectly.

I recently learned about the enzyme `wrapper.instance()` method, which **returns the component instance inside of the wrapper**. Getting access to this instance allows you to directly invoke component methods, instead of resorting to event simulation to indirectly trigger them.

Access to the instance also allows you to spy on component methods using `jest.spyOn()`, which can be useful to ensure that complex interactions between helper methods occur as expected.

## Example

Here is a `home` component, which contains a button and a piece of counter state.

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
        <div>{this.state.counter}</div>
        <button onClick={() => this.incrementCounter(two)} >Increment</button>
      </div>
    )
  }
}

{% endhighlight %}

I'll show a few different ways to test the `incrementCounter` method of the component above, both indirectly and directly.

## Testing incrementCounter Indirectly

An indirect way of testing the `incrementCounter` method would be to **simulate a click event on the button to which the method is attached** using the enzyme wrapper event simulation functionality.

{% highlight javascript %}
describe('indirectly testing "incrementCounter" through click simulation', () => {
  it('should update the count by 1 when invoked by default', () => {
    const wrapper = shallow(<Home />);
    expect(wrapper.state('counter')).toBe(0);
    wrapper.find('button').simulate('click');
    expect(wrapper.state('counter')).toBe(1);
  });
  it('should add two to the count when the "two" value is true', () => {
    const wrapper = shallow(<Home two />);
    expect(wrapper.state('counter')).toBe(0);
    wrapper.find('button').simulate('click');
    expect(wrapper.state('counter')).toBe(2);
  });
});
{% endhighlight %}

When indirectly testing a component method **you have little control over how the method is invoked**. This often requires configuring state + props in different ways before indirectly triggering the method to make sure all branches were fully exercised. In the tests above, I set up two different versions of the component (one with and one without the `two` prop) so I could hit both branches of the `incrementCounter` method.

## Testing incrementCounter Directly

Now let's see what it looks like to test the `incrementCounter` method directly, using the component instance returned by [`wrapper.instance()`][instance]{:target="_blank"}.

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

Clearly there is a lot more **control** here: invoking an instance method directly allows you to call the method at any time. This is particularly convenient when methods are pure functions (i.e. they do not depend on any external state to behave correctly) since they can be fully exercised by invoking them with different parameters.

If the `incrementCounter` method did not accept the argument `two`, and instead relied on it being available on props, I would still need to configure my wrapper to make sure props are correctly set before directly invoking. Just another argument for **keeping functions pure** for ease of testing.

## Spying on incrementCounter

Finally, it is also possible to use [`jest.spyOn()`][spy]{:target="_blank"} to spy on component methods through the component instance returned from `wrapper.instance()`.

{% highlight javascript %}

describe('spying on "incrementCounter" method', () => {
  it('should call incrementCounter when the button is clicked', () => {
    const two = true;
    const wrapper = shallow(<Home two />); //passing the "two" prop to test if it is properly passed to onClick handler
    const instance = wrapper.instance();
    jest.spyOn(instance, 'incrementCounter');
    wrapper.find('button').simulate('click');
    expect(instance.incrementCounter).toHaveBeenCalledWith(two);
    expect(wrapper.state('counter')).toBe(2);
  });
});

{% endhighlight %}

This is yet another useful way of testing to make sure only the component methods that you expect to be called are called in response to a simulated event from the component. Spies also allow you to confirm that methods were **called with the correct arguments**.

If you are used to `jasmine` or `sinon` spies, you might expect `jest.spyOn()` to automatically replace the component method with mock implementation. **This is not the default behavior of `jest.spyOn()`**. If you want to provide a new implementation to a function you are spying on in this manner, try the following:

{% highlight javascript %}

jest.spyOn(instance, 'incrementCounter').mockImplementation(() => {
  // custom implementation here
})

// or if you just want to stub out the method with an empty function

jest.spyOn(instance, 'incrementCounter').mockImplementaiton(jest.fn())

{% endhighlight %}

Learning about `wrapper.instance()` definitely allowed me to get more granular with my testing, and allowed me to simplify some of my test setup for more complicated scenarios that I was previously testing indirectly through event simulation.

[github]: https://github.com/bambielli/testing-react-component-methods
[instance]: http://airbnb.io/enzyme/docs/api/ReactWrapper/instance.html
[spy]: https://facebook.github.io/jest/docs/en/jest-object.html#jestspyonobject-methodname-accesstype
