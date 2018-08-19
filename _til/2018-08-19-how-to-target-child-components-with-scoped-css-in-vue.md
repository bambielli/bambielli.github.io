---
title:  "How to Target Child Components with Scoped CSS in Vue"
date:   2018-08-19 11:36:00
category: til
tags: [vue, scoped, styles, css, deep, selector]
---

TIL how to target a child component with a CSS selector from a parent component that is using `Scoped CSS`.

## What is Scoped CSS?

[`Scoped CSS`][scoped]{:target="_blank"} is a mechanism in `vue.js` single file components that prevents styles from leaking out of the current component and affecting other unintended components on your page.

{% highlight vue %}
// Parent.vue //
<template>
  <div class="example">I'm a {{message}}</div>
</template>

<script>
  export default {
    name: 'Parent',
    data() {
      return {
        message: 'parent'
      }
    }
  };
</script>

// Notice the `scoped` attribute on the style tag.
<style scoped>
  .example {
    font-size: 1.5em;
    background-color: blue;
  }
</style>

{% endhighlight %}

Scoped CSS works thanks to some `PostCSS` magic: during preprocessing of your vue.js component, any selectors are transpiled to have a corresponding `data-xyz` attributed added to them. These data attributes are also added to elements in your component's template.

So after transpilation, the component above might look something like this (data selectors are randomly generated):

{% highlight vue %}
// Parent.vue (transpiled) //
<template>
  <div class="example" data-123>I'm a {{message}}</div>
</template>

<script>
  export default {
    name: 'Parent',
    data() {
      return {
        message: 'parent'
      }
    }
  };
</script>

<style scoped>
  .example[data-123] {
    font-size: 1.5em;
    background-color: blue;
  }
</style>

{% endhighlight %}

The addition of the data attribute to selectors and elements in the template makes selectors specific to the component in which they are defined, preventing style bleed to other components in your app that might use the same class names.

## How to Target Children: the /deep/ selector

This is great for elements that are "owned" by the current component, but what if you need to modify the styles of a child component? If you try to target a child component with a selector, **the data attribute will prevent that child from being selected**.

In order to target children with CSS selectors in a parent using `Scoped CSS`, you need to add the `/deep/` selector to the beginning of your selector statement.

This will let your select statement correctly target a piece of a child component from a scoped parent.

Example:

{% highlight vue %}
// Child.vue //
<template>
  <div class="child">I'm a {{child}}</div>
</template>

<script>
  export default {
    name: 'Child',
    data() {
      return {
        message: 'child'
      }
    }
  };
</script>

<style scoped>
  .child {
    background-color: green;
  }
</style>

// Parent.vue //
<template>
  <div class="example">I'm a {{message}}</div>
</template>

<script>
  export default {
    name: 'Parent',
    data() {
      return {
        message: 'parent'
      }
    }
  };
</script>

<style scoped>
  .example {
    font-size: 1.5em;
    background-color: blue;
  }
  /deep/ .child {
    background-color: blue;
  }
</style>

{% endhighlight %}

In the above example, `/deep/ .child` selector will target the `.child` class in the child component with a more specific selector, which will cause the background color to be blue instead of its default of green.

Scoped CSS reminds me a lot of [CSS Modules][modules]{:target="_blank"} which also provides similar scoping mechanisms to prevent style bleed/conflicts in a project.

[scoped]: https://vue-loader.vuejs.org/guide/scoped-css.html
[modules]: https://github.com/css-modules/css-modules

