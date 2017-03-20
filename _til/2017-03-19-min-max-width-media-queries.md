---
title:  "min-width vs max-with in Media Queries"
date:   2017-03-19 22:40:00
category: til
tags: [css]
---

TIL a succinct way to remember the difference between using `min-width` and `max-width` in a media query.

I spent part of today fixing up the mobile view of a few of my site's components. I wound up needing to write a media query for the pagination component at the bottom of my homepage: a simplified version of the pagination component renders when the screen is below 600px, and a more complex version renders at larger widths.

The queries I wound up using look like this:

{% highlight css %}
$small : 600px;

.show-on-mobile {
  @include breakpoint(min-width $small + 1) {
    display: none;
  }
}

.hide-on-mobile {
  @include breakpoint(max-width $small) {
    display: none;
  }
}
{% endhighlight %}

Note: the [breakpoint][breakpoint]{:target="_blank"} mixin makes writing media queries less verbose and more semantic.

Here are english translations of min-width and max-width:

`min-width`: applies styles at screen widths greater than or equal to the specified pixel value. In other words, this represents the **minimum screen width at which a style is applied**. Elements with the class `.show-on-mobile` will have the style `display: none;` applied at screen widths greater than or equal to 601px.

`max-width`: applies styles at screen widths less than or equal to the specified pixel value. In other words, this represents the **maximum screen width at which a style is applied**. Elements with the class `.hide-on-mobile` will have the style `display: none;` applied at screen widths less than or equal to 600px.

Notice that `.show-on-mobile` is set to apply at screen widths 601px and greater... initially I had both of these set to 600px, but since min and max width both apply at values equal to the specified pixel width, **both MQs were applying at exactly 600px and the pagination buttons were disappearing.**

Changing `.show-on-mobile` to 601px makes the mobile view show at 600px, then hide at 601px when the non-mobile view is unhidden (and .hide-on-mobile no longer takes effect).

The behaviors of `min-width` and `max-width` were reversed in my head when I started writing my MQs. i.e. it is somewhat counter intuitive to me that a `min-width` query represents styles that will be applied at widths greater than the specified pixel value. Thinking about these properties in terms of "min width at which a style is applied" and "max-width at which a style is applied" makes more logical sense to me.


[breakpoint]: http://breakpoint-sass.com/
