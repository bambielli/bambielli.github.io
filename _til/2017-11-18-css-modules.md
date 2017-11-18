---
title:  "Setting Up CSS Modules"
date:   2017-11-18 13:35:00
category: til
tags: ["css", "webpack", "react", "create react app", "cra", "modules", "css modules"]
---

TIL how to set up CSS modules for a React application that uses webpack.

## What are CSS Modules?

Has this ever happened to you:

You're working on a large application with many pages, containers, components...

You want to define a class called `header` for a component, which should have blue text. You define it thusly:

{% highlight css %}

.header {
  color: blue;
}

{% endhighlight %}

When you apply the class to your new element **headers for all elements across the application become blue as well**.

This is due to `global namespace conflicts`, a common problem in css, where classes with the same name will override each other based on the order in which they are defined/loaded (the "cascading" part in "cascading style sheets").

To combat this, you adopt a naming convention like [`BEM`][bem]{:target="_blank"} to ensure global namespace conflicts won't happen. Depending on the size of your app, the BEM naming convention can get quite... verbose.

{% highlight css %}

.menu--item--header--initial--primary--first--organic--left--highlight {
  color: blue;
}

{% endhighlight }

[CSS modules][css]{:target="_blank"} provide a **build-time workaround** for the age old problem of namespace conflicts with css classes. The CSS module system allows you to write short semantic class names by swapping class names with unique identifiers that will not cause conflicts in your app.

## How do you configure it?

If you are developing using create react app in an ejected state, the configuration is trivial. Modify your `webpack.config.dev.js` css-loader configuration so it looks like the following:

{% highlight javascript %}

  {
    loader: require.resolve('css-loader'),
    options: {
      importLoaders: 1,
      modules: true,
      localIdentName: "[name]__[local]___[hash:base64:5]"
    },
  },

{% endhighlight %}

And the following to your `webpack.config.prod.js`

{% highlight javascript %}

  {
    loader: require.resolve('css-loader'),
    options: {
      importLoaders: 1,
      modules: true,
      minimize: true,
      sourceMap: true,
    },
  },

{% endhighlight %}

It's as simple as that! To repeat: if you are using create-react-app you will need to eject in order to add the necessary config.

## Using modules

In a React app, using modules is also quite simple and fits well in the componentized world that React encourages. **Each component can have its own CSS file** with locally scoped styles to that component. This makes a component even more modular, as style, display, and logic are all tied nicely in to one package.

Here is some javascript for a high-level component that uses CSS modules

{% highlight javascript %}
import React from 'react';
import styles from './app.css'; // Import relevant styles, and name the import `styles`

export default function App() {
  return(
    <div className={styles.app}> //note how we reference styles from the `styles` object.
      <div className={styles.topHeader}> // styles must not use dashes in their names, and instead should use camelCase.
        Hi mom!
      </div>
    </div>
  )
}

{% endhighlight %}

And that's about it! If you render this component in the browser, you will see that the classes have been renamed to something like `App__topHeader__b9a112d`. That's the module system at work: by renaming classes in this way, global namespace conflicts become a non-issue. Furthermore, you don't have to adopt a convention like BEM when naming your classes, since the module system essentially does this for you.

We started using CSS Modules a few months back at Expedia, and it made writing CSS fun again. I highly recommend it!

[css]: https://github.com/css-modules/css-modules
[bem]: http://getbem.com/introduction/
