---
title:  "CSS Inline Transformations"
date:   2017-01-16 14:56:00
category: til
tags: [css, jekyll, liquid, animation]
---

TIL that inline elements can’t have transformations applied to them in CSS.

To help ensure I follow through on my resolution to [reach 100 TILs in my TIL bank][til]{:target="_blank"} by the end of 2017, I decided to add a little counter to my page to keep the number remaining front and center throughout the year. Check out the [TIL page][home]{:target="_blank"} to see the counter.

## Jekyll (and Liquid) cheat sheet

The first task was getting the size of the array of TILs I had already written. I had never done computation in Jekyll before, so I needed to do a bit of research. Luckily I found this [cheat sheet by Github user smutnyleszek][cheat]{:target="_blank"} that got me on the right track for defining local variables in a page, getting the size of an array, and doing simple calculations like subtraction.

I created a variable called `numTil` and assigned it to the number of TILs currently displayed on the page (the number I had already written). I then calculated the `numToGo` by subtracting numTil from 100. Performing subtraction in Jekyll templates isn't as easy as `100 - numTil`. You actually have to pass the value to a filter that does the subtraction for you: i.e. `{% assign numToGo = 100 | minus:numTil %}`. Weird.

## Polish && Problems

Now that I had my value calculated and displayed on my page, I wanted to add a little Easter egg animation for fun: an on-hover CSS animation that would cause the number to do a flip. I initially set up the page to be an h3 title containing a span with an id of "resolution" which would contain my calculated number, allowing me to target the span with an animation.

{% highlight html %}
  <h3>
    Number of TILs remaining to meet 2017 goal: <span id="resolution"> {{numToGo}} </span>
  </h3>
{% endhighlight %}

And the CSS animation:

{% highlight css %}

#resolution {
  -webkit-transition: -webkit-transform .7s ease-in-out;
          transition:         transform .7s ease-in-out;
}
#resolution:hover {
  -webkit-transform: rotate(360deg);
          transform: rotate(360deg);
}

{% endhighlight %}

However, I found that my animation wasn't working on my span :(

## Solution

After digging in to the issue for a while, I stumbled upon this definition of a [transformable element][transformable]{:target="_blank"} from the W3C spec. An element with display of inline (like span) is not covered under the list of transformable element types! This answered my question, now on to the fix.

Changing the span to a div "fixed" the issue (since divs have a display of block) but I wanted my number aligned inline with the text in my h3. Changing to a div also caused the container to stretch across the whole page, making the entire horizontal space on the same line as my number "hoverable". The animation also looked bad, since it was spinning around the entire div space.

Changing the display to "inline-block" gave me exactly the result I was looking for, making the element both transformable and inline with the rest of the text in the line.

{%raw%}</TIL>{%endraw%} (one down!)

[cheat]: https://gist.github.com/smutnyleszek/9803727
[til]: /posts/2016-12-30-2016-recap#blogging
[home]: /til
[transformable]: https://www.w3.org/TR/css-transforms-1/#transformable-element

