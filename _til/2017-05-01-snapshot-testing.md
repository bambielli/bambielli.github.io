---
title:  "Snapshot Testing with Jest"
date:   2017-05-01 22:59:00
category: til
tags: [javascript, testing, react, jest]
---

TIL how to perform snapshot testing using Facebook's React unit testing framework `jest`. It is somewhat different than traditional visual regression testing... read on to learn more!

[Visual regression testing][screenshot]{:target="_blank"}, also known as screenshot testing, involves taking automated screenshots of views under test, and performing a pixel comparison against a stored screenshot of the view to check for regressions. These tests can be quite brittle, are prone to false negatives, and are difficult to maintain...

[Snapshot testing][snapshot]{:target="_blank"} is a response to visual regression testing by Facebook, and was released in July of 2016 with Jest v14.0. Instead of storing image screenshots of your UI and performing a pixel comparison, [jest][jest]{:target="_blank"} stores a version of the rendered output of the react component under test in a "snapshot file". These snapshot files are committed to the repository alongside other UI tests, and are used by the testing framework to compare against rendered component output in future test runs.

If nothing changes in the rendered output of a component between test runs, the snapshot test passes. If a change is detected, the test will fail and Jest will show a diff between the expected and actual outputs in the console.

Here is an example of what a snapshot test looks like (taken from the Jest snapshot documentation):

{% highlight javascript %}

import React from 'react';
import Link from '../Link.react';
import renderer from 'react-test-renderer';

it('renders correctly', () => {
  const tree = renderer
    .create(<Link page="http://www.facebook.com">Facebook</Link>)
    .toJSON();
  expect(tree).toMatchSnapshot();
});

{% endhighlight %}

And here is what the corresponding snapshot file looks like:

{% highlight javascript %}

exports[`renders correctly 1`] = `
<a
  className="normal"
  href="http://www.facebook.com"
  onMouseEnter={[Function]}
  onMouseLeave={[Function]}
>
  Facebook
</a>
`;

{% endhighlight %}


## Additional Things to Note

**Snapshot tests are not flakey:** Unlike visual regression tests, snapshot tests do not perform pixel comparisons of the rendered output of the view under test. Instead, Facebook suggests using a [test renderer][test]{:="_blank"} to generate the equivalent rendered output for a component or view. Using a test renderer **does not require a browser or browser-like environment.** This keeps tests fast and makes tests more reliable, as the test renderer deterministically renders component output. NOTE: Be sure to use mocks when rendering component output, particularly if your component relies on functions or services that can change over time (i.e. Javascript Date objects). This will ensure deterministic snapshots are rendered.

**Snapshot tests are a compliment to traditional unit tests, not a substitute:** A robust unit test suite should still be used to verify business logic in your code. Snapshot testing should be used to verify portions of your UI that would otherwise be difficult to test (i.e. the rendered output of React components under certain circumstances).

**Snapshot files are small** so you can have many different variations of snapshots for a particular component without worrying about your repository growing in size.

**Snapshot files are easy to update.** If the rendered output of a component was purposefully changed, it is simple to update snapshots with the new expected output. Just run `jest --updateSnapshot` and all snapshots will be updated against the latest component versions.

**Snapshot testing is not recommended for TDD:** i.e. manually creating snapshot files first, then writing code to that "spec". Snapshot tests are intended to detect if the rendered output of a component has changed, rather than provide a spec to which code should be written.

Snapshot testing seems to be a great way to test otherwise difficult to test portions of the UI (e.g. react components) with the speed and fidelity of unit tests. I'm excited to start incorporating snapshot testing into my projects!

[jest]: https://facebook.github.io/jest/
[snapshot]: http://facebook.github.io/jest/docs/snapshot-testing.html#snapshot-testing-with-jest
[screenshot]: https://visualregressiontesting.com/
[test]: https://www.npmjs.com/package/react-test-renderer
