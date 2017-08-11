---
title:  "Composing classes using css-modules"
date:   2017-08-11 13:22:00
category: til
tags: [css, front-end, composes, css-modules]
---

TIL how to compose classes together using the `composes` keyword provided by `css-modules`. It has changed my world.

[`css-modules`][mod]{:target="_blank"} provide a system for writing css classes that are scoped locally to a particular component or file.

Since all classes are scoped locally (through transpiling of class names into unique names at build time) you can also use the same class names across multiple files without the worry of accidentally overriding styling in other parts of your project. Webpack's `css-loader` does this transpilation step for you automatically.

**I really really like css-modules.** The notion of modularized styling fits nicely with a React component-based architecture: each component that you build can have its own styles and classes that do not bleed out of the component boundary.

## The `composes` keyword

I've discussed the benefits of [**favoring composition over inheritance**][blog] in my previous post on the `decorator` design pattern. The same adage holds true in the context of css-modules!

css-modules offers a way of composing multiple classes together to form new classes, via the `composes` keyword. Similar to how you might compose two java classes together to build more complex objects out of other objects in a flexible way, the `composes` keyword allows you to build a new class out of styles defined by other pre-defined classes.

For example:

{% highlight css %}

.classA {
    background-color: green;
    color: white;
}

.classB {
    composes: classA;
    color: blue;
}

{% endhighlight %}

`.classB` above overrides the color property from `.classA` with 'blue', but will still have a background-color of green. If `.classA` changes its background-color to 'red', this change will be reflected in `.classB` through class composition.

**You can import styles from other modules, and compose multiple classes together while defining new classes.** This flexibility can lead to more efficient and reusable css class design.

[mod]: https://github.com/css-modules/css-modules
[blog]: /posts/2017-04-02-decorator/