---
title:  "CSS Positioning"
date:   2017-02-26 19:51:00
category: til
tags: [css]
---

TIL (reviewed) the differences between the 4 CSS position property values: `static`, `fixed`, `relative`, `absolute`. 

I've been TA'ing a Web Development Bootcamp over the past month at Northwestern, and we have been covering a lot of HTML / CSS fundamentals that I often forget. One such example is the differences between the various css position values, and when to use each. 

### The 4 Position Values

The four position values are as follows:

- `static`: no special positioning behavior. Positioned according to normal "flow" of the page. Not affected by explicit position values applied by `top`, `bottom`, `left` or `right`.

- `fixed`: element is positioned relative to the viewport. Stays "fixed" in the same place, even as the window scrolls.

- `relative`: element is positioned in layout as if it were `static` (normal flow), then is adjusted from it's normal location per any explicit coordinates applied by `top`, `bottom`, `left`, `right`. A gap is left in the layout for the element's original location.

- `absolute`: element is positioned in layout relative to the closest positioned ancestor (e.g. relatively positioned ancestor). No gap is left in the layout for the element's "normal" location. If there is no relatively positioned ancestor, position it relative to "body". 

### absolute Vs. relative 

The tricky one for me to remember is the difference between `relative` and `absolute` positioning. 

They both honor explicit position values passed by `top` and the like, but `relative` leaves a gap in the layout for it's "normal" location before it is adjusted by explicit positioning. 

Coordinates applied to an `absolute` positioned element are relative to the nearest `relative` container. So if an `absolute` div is contained by a `relative` container, coordinate (0,0) of the aboslute div will be the top left corner of the container, not the top left corner of the page!

I cooked up a [quick example][example]{:target="_blank"} that illustrates absolute positioning. Check out the link.

This is the first example that I've hosted on github pages, which makes hosting small static sites (including this blog) a breeze. 

### Bonus TIL

I forgot that the default position value of elements is `static`. Good to remember.

TILL (double learned!)

[example]: http://www.bambielli.com/absolute-positioning-example/
