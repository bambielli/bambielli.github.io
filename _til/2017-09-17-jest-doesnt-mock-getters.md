---
title:  "Jest automock doesn't mock getters"
date:   2017-09-17 18:50:00
category: til
tags: [javascript, testing, test, jest, react]
---

TIL that Jest module automocking does not mock the getter / setter properties of the target module.

## Jest genMockFromModule

The Jest testing framework provides many nice utilities out of the box (see my previous TIL on Jest snapshots) one of which is module mocking. This is used to isolate the component under test from its dependencies.

The `genMockFromModule` method makes mocking dependencies a breeze: just create a file of the same name in an `__mocks__` folder, call `genMockFromModule` with the module's path, and export the mocked module from the file. When you call `jest.mock('..path/to/module')` in your test, the mocked version of the module will be fetched from the `__mocks__` folder, and can be further configured on a test case by test case basis.

The following example creates a mock out of a Mobx store, which is then used in the test file.

{% highlight javascript %}
// file __mocks__/featureStore.js

const mockedFeatureStore = jest.genMockFromModule('../featureStore.js');
export default mockedFeatureStore;
{% endhighlight %}

{% highlight javascript %}
// file __tests__/feature.js
import FeatureStore from '../featureStore.js';
jest.mock('../featureStore.js'); // grabs the mocked version of the store

const featureStore = new FeatureStore('hello', 'world'); // FeatureStore is now the mocked version!

/* ...start testing with the featureStore instance */

{% endhighlight %}

Any methods on the store above will be automatically mocked by Jest, giving you, the tester, full control over how dependencies respond to your module under test.

## Mocking Modules with Getters

Something I discovered during a recent round of testing, was that **object getter properties are not swapped out with mocked versions by genMockFromModule**.

Object getter properties look like regular object properties, but are computed values with logic behind them. To get a fully isolated version of your dependency, it is necessary to provide mock values for these properties when they are accessed by the code under test.

Since getters just look like normal object properties, automocking will miss them... **automocking only looks for functions on an object**, and will expect the tester to configure any additional values necessary when using the mock during testing.

There are two ways I came up with to do this, one easy and one hard.

### Option 1: Use Object.defineProperty

If you truly want to mock the getter aspect of your dependency, you can provide a mock getter using `Object.defineProperty`. For example, if you want to mock a property "isLoading" on your object that has a getter behind it, you could do the following:

{% highlight javascript %}

Object.defineProperty(mockObject, "isLoading", {
  get: () => {
    return true;
  }
});

{% endhighlight %}

After defining this property on your mock, when your component accesses the property "isLoading" during tests, the function defined under the `get` attribute will execute and return true.

### Option 2: Just assign a value

The second option is to simply assign a value to the property in your test.

{% highlight javascript %}

mockObject.isLoading = true;

{% endhighlight %}

**This is by far the simpler approach.** Since the object that is using the mock doesn't care whether or not a getter is being executed behind the scenes, just assigning a value to the object property is enough.
