---
title:  "How to Automock node_modules with jest"
date:   2017-12-14 13:35:00
category: til
tags: ["jest", "mocking", "mocks", "automock", "test", "testing", "unit", "facebook", "sentry.io"]
---

TIL how to easily node_modules in unit tests using the jest testing framework from Facebook.

At work, we have been implementing client-side error logging using the service [sentry.io][sentry]{:target="_blank"}.

Sentry provides a javascript package called `raven-js` which provides a few methods for easily pushing error messages to the service.

Unfortunately, after implementing logging throughout our code using the Raven package, our unit tests began to fail... **`Raven` was throwing errors as it was attempting to push errors to sentry from the test environment.**

Clearly this is something that we should be mocking for testing purposes, but what is the easiest way to mock a node_module using `jest`?

## Automocking node_modules

[Jest recommends][jest]{:target="_blank"} adding a `__mocks__` folder to the root of your repository, and keeping mocks for node_modules there.

By default, when jest detects a node_module import during tests, it will automatically look in the `__mocks__` folder for a mock that matches the name of the module being imported, and preferentially use the mock over the actual implementation.

In our case of mocking the Raven library for tests, we created our mock as follows:

{% highlight javascript %}
// /__mocks__/raven-js.js
const Raven = requre('raven-js');
jest.genMockFromModule('raven-js');
module.exports = Raven;
{% endhighlight %}

Simple, right?

The `jest.genMockFromModule` method automocks the provided module by substituting its public methods with `jest.fn()` implementations. You can still confirm in your unit tests that the correct methods were called using `expect(Raven.methodName).toHaveBeenCalledWith(param)`.

You can extend the mock with additional fake implementation as necessary since it is just a regular ol' jest `manual mock`.

I found this to be the quickest way to mock the new node_module throughout my unit tests, while touching the fewest number of files possible.

[sentry]: https://sentry.io/welcome/
[jest]: https://facebook.github.io/jest/docs/en/manual-mocks.html
