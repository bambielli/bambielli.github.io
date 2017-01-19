---
title:  "The Console Object && CSS Specificity"
date:   2017-01-18 22:25:00
category: til
tags: [console, devtools, chrome, css]
---

TIL (more like relearned) CSS specificity rules, and some additional methods on the console object other than `console.log`.

We had a UI Community of Practice meeting today at Expedia. There were 2 speakers: the first spoke about CSS specificity rules, and the second spoke about the console object and some of the additional methods on it past just `console.log`

### CSS Specificity

CSS specificity dictates the order of precedence that CSS is applied to elements in the DOM. If two CSS rules apply to the same element, the one with the higher specificity will win out and overwrite the previous rule. Specificity is often the reason that CSS does not work the way a developer/designer is expecting, and can lead to abuse of `!important` throughout the codebase.

The rules are as follows:

 - tags and pseudo-elements --> specificity of 0,0,0,1
 - classes and pseudo-classes --> specificity of 0,0,1,0
 - IDs --> specificity of 0,1,0,0
 - inline styles --> specificity of 1,0,0,0

Often specificity numbers are written without the commas, but I think the commas add clarity: for example, if you wind up having a crazy selector with 12 classes, that does not "roll over" in to the 3rd digit and overwrite the specificity of a single ID. The commas make this distinction clearer. Using `!important` still usurps all, however, and therefore should be used sparingly.

A general rule we came up with was, use the least specific selector to apply a rule. The less specific the selector, the less DOM the browser will need to traverse to find the element to which the rule should be applied (leading to more performant rendering).

Another common-sense rule was, do not put classes or elements before the ID in your selector. Since IDs are the most specific selector available, and they are also unique to a document, any classes or elements applied before an ID are wasted work for the browser. When constructing a selector, just jump right to the ID!

 ### The Console Object

The `console` object is so much more than just `console.log`!

 - `console.warn`, `console.info`, `console.error`, and `console.debug` are all alternative ways to log information to the console. While log and `debug` are essentially equivalent in behavior, `warn` highlights your log output in yellow and provides a stacktrace. `error` does the same but highlights in red. `info` puts a little blue info icon next to your log output in the console. Nifty!

 - On the topic of logs, all logging methods on the console object can accept multiple arguments, which will be attached to the end of the log output. So you can have a log statement like `console.log('user object is: ', user)` to log a particular object in the same line as a statement that clarifies what that object is and where it is coming from in the code.

 - Logs also accept placeholders like `%s` and `%d` in strings: i.e. `console.log('my name is %s!', 'brian')` will log "my name is Brian!".

 - `console.time` and `console.timeEnd` offer a way to time how long the execution of a particular block of code takes. These methods know when to stop timing based on a common tag that you pass in (i.e. "timer1"). This way you can have multiple timers going at once.

 - `console.profile` and `console.profileEnd` can be used in similar fashion to `console.time`, however they will not only time, but will also create a profile report for the code block they wrap. That profile report will show up in the profiles tab in the chrome dev tools, and can be downloaded and saved for later.

{%raw%} </TIL> {%endraw%}

