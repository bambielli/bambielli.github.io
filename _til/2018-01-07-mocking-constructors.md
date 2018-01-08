---
title:  "How to Use Jest to Mock Constructors"
date:   2018-01-07 16:59:00
category: til
tags: [test, testing, unit, unit test, jest, mocks, manual mocks, manual, automock, genMockFromModule, constructor, module]
---

TIL how to mock the constructor function of a node_module during unit tests using `jest`.

[As noted in my previous post,][previous] `jest` offers a really nice automocking feature for node_modules. Automocking the module will suffice for most testing scenarios you come up with, since it allows you to separate behavior of the module from the way your application code uses it.

I ran into a testing scenario where straight automocking was not sufficient for what I needed to test.

We recently integrated the `bunyan` structured logger into our app, and unlike the `Raven` service provided by sentry for client-side logging, **bunyan provides a constructor for initialization, which returns a configured logger.**

{% highlight javascript %}
// logger.js
const Bunyan = require('bunyan');

const bunyanLogger = new Bunyan({
  ...configuration,
});

const logger = {
  trace: (msg) => {
    bunyanLogger.trace(msg);
  }
}
module.exports = logger;

{% endhighlight %}

This proved problematic for testing purposes... I wanted to mock the bunyan logger for unit testing, but if I used `jest.genMockFromModule('bunyan')` alone, **the constructor function provided by the module would also be mocked.**

This would cause my smoke tests for the logger.js file to fail, since the bunyanLogger would just be an empty object.

What I really needed was a way to mock the bunyan constructor, and have it return an object with the same shape as the unmocked version... but how?

Jest provides a method called [`mockImplementation`][impl]{:target="_blank"} that allows you to provide new implementation for a mock that has already been created. This was necessary in our case over just assigning the bunyan module to a new `jest.fn()` with mock implementation, because we needed access to some constants on the mocked bunyan module in our configuration step.

We couldn't overwrite bunyan **entirely** with a reference to a `jest.fn()`, instead we needed to change one part of the mocked module with new implementation. This is what `mockImplementation` allows you to do.

{% highlight javascript %}
// logger.test.js
import Bunyan from 'bunyan';
import logger from '../logger';

jest.genMockFromModule('bunyan');
jest.mock('bunyan');

const mockBunyanLogger = {
  trace: jest.fn()
};

Bunyan.mockImplementation(() => mockBunyanLogger);

describe('logger', () => {
  it('should call the trace method of the mockBunyanLogger with the msg passed' () => {
    logger.trace('msg');
    expect(mockBunyanLogger.trace).toHaveBeenCalledWith('msg');
  });
});

{% endhighlight %}

The `mockImplementation` step says "when the Bunyan constructor is called, return this mockBunyanLogger object in place of the original". Since the mockBunyanLogger object reference is in my test file, I can reference the mock methods in my expectations.

The astute reader will notice that I'm relying on a particular structure / method name being returned from the bunyan constructor (the `mockBunyanLogger` makes this assumption).

If the structure returned from Bunyan ever changed in a breaking way, my tests as currently written would not fail.

Instead of relying on a particular structure returning from bunyan and hard coding that in to my tests, I could consider unmocking the bunyan module, making a call to the constructor with valid configuration to get the shape of the logger, and then using something like [`jest-mock-object`][obj]{:target="_blank"} all of the object methods with `jest.fn()` for spying purposes.

This would allow me to catch any changes in my node_module constructor contract, while also reducing the number of assumptions I make in my code.

[previous]: /til/2017-12-14-mocking-node-modules-with-jest
[obj]: https://www.npmjs.com/package/jest-mock-object


