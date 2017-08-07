---
title:  "Console.log Output is Sometimes Incorrect in Chrome"
date:   2017-01-31 21:54:00
category: til
tags: [chrome, devtools, javascript]
---

TIL that the output of console.log in chrome can be misrepresentative of the actual state of an object at the point of logging.

During a particularly grueling debugging session that involved a misconception of mine about [how the spread operator copied properties][til]{:target="_blank"}, I ran in to some strange observed behavior by chrome's `console.log` method that made me think I was going crazy.

## The Observed Behavior

I was mutating an object, running some validation on it, then mutating it back to its original state. I was trying to log the intermediate state of this object to be sure it was being mutated properly. However, the intermediate logs were not what I was expecting...

{% highlight javascript %}
  const wallet = {value: "123"};
  console.log(wallet); // {value: "123"}

  wallet.value = +wallet.value; //converts to integer
  console.log(wallet); // {value: "123"} <-- WHAT THE HECK???

  /* validation omitted */

  wallet.value = wallet.value.toString();
  console.log(wallet); // {value: "123"}

{% endhighlight %}

I called a coworker over to my desk at this point, so he could see what was happening and confirm that I wasn’t going crazy.

The second log statement should have logged the object with the value property as a number, not a string, since I had just converted it the line prior.

Next, I opened the debugger in chrome so I could step through the code... I accessed the value directly after it was mutated to a number, **and it did indeed return as a number.**

Logging the value in the following line, however, **returned the object with string output**.

My validation logic was operating correctly, so I know the value was actually being passed to it as an integer, but it still didn't explain why the logging was mismatched to reality.

## The Resolution?

My coworker and I stumbled across this [stack overflow post,][so]{:target="_blank"} where another person was having similar problems. It doesn't really explain why the issue is happening, but it does point out that `console.log` **is somewhat lazy in its evaluation, and will log the last state of an object reference.**

Another weird caveat... if the same code I posted above is executed in Firefox, the `console.log` statements will log the correct intermediate mutated value! I guess there must be some difference between webkit and gecko implementations of the console object, or at least the resolution of logs.

Not a very satisfying answer as to *why* this occurs, but still a TIL none the less!

[til]: /til/2017-01-29-spread-operator-deep-copy
[so]: http://stackoverflow.com/questions/4057440/is-chromes-javascript-console-lazy-about-evaluating-arrays

