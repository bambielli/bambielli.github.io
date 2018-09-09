---
title:  "Move jest.conf Out of Root Directory"
date:   2018-09-09 10:36:00
category: til
tags: [jest, react, javascript, testing, jest.conf, jest.conf.js, package.json, test]
---

TIL how to move a `jest.conf` file out of the root directory for a project, while still allowing it to respect all of the relative pathing defined in the config.

[Jest has a ton of configuration options][config]{:target="_blank"}, which can be stored either under a `jest` key in in your package.json, or in a separate jest.conf file.

We like to keep our package.json dedicated just to npm scripts, dependency versions, and application metadata, so we went the route of creating a separate `jest.conf` file in our latest project.

Several jest configuration options involve specifying file paths for certain transformations to be applied. For example:

{% highlight javascript %}
// jest.conf
{
  "transform": {
    "^.+\\.(js|jsx)$": "<rootDir>/node_modules/babel-jest"
  },
}
{% endhighlight %}

The transform above tells jest to run files that end in .js or .jsx through babel, so they are transpiled  to node-compatible javascript.

Notice the `<rootDir>` prepended to the babel-jest path above... `<rootDir>` allows you to specify relative paths in your jest config, which makes executing tests in different environments (local, jenkins, different machines) much simpler.

The name `<rootDir>` makes it seem like it would always reference the project root, right?

**WRONG!**

`<rootDir>`**defaults to the directory in which your jest.conf file exists.**

When we moved our jest.conf file to a `config/` directory, our transformations started breaking. Jest couldn't locate the `babel-jest` package anymore.

After doing some digging, we found a [jest configuration option for updating the `rootDir` to point at root][rootDir], even from inside of a folder.

We updated our config to the following:

{% highlight javascript %}
// jest.conf from inside `config/` directory
{
  "rootDir": '../',
  "transform": {
    "^.+\\.(js|jsx)$": "<rootDir>/node_modules/babel-jest"
  },
}
{% endhighlight %}

Since our jest.conf file is one directory down from root, we needed to configure `<rootDir>` to point back to the root from inside of this directory (e.g. go one level up). Without doing this, `<rootDir>` was referencing everything relative to the `config/` directory.

Hope this saves someone some time in the future!

[config]: https://jestjs.io/docs/en/configuration.html
[rootDir]: https://jestjs.io/docs/en/configuration.html#rootdir-string
