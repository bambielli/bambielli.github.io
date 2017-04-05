---
title:  "'let' transpilation in Babel"
date:   2017-04-04 22:44:00
category: til
tags: [javascript, babel]
---

TIL how Babel transpiles variables defined with `let` to their ES5 `var` equivalents, while still maintaining the same block scoping rules that apply to `let`. 

Yesterday, during office hours, I was showing some students the [Babel REPL.][babel]{:target="_blank"} We were playing around with ES6 `let` variable definitions, and reviewing how unlike their `var` counterparts **variables defined with `let` stay predictably scoped to the block in which they are defined.**

I was interested to see how Babel would transpile `let` variable definitions to ES5 syntax. I created an `if` block and created a `let` variable inside of the body. My expectation was that the `let` variable should be scoped to the if body, and should be inaccessible outside of the block. 

<figure>
  <img src="/assets/images/LetBlockOriginal.jpg">
  <figcaption>Initial `let` Block test</figcaption>
</figure>

The transpilation seemed pretty predicatable here: until ES6, the only type of variable definition available was `var`, so the `let` inside of my if statement was simply transpiled to `var`. No magic here!

Check out what happened when I tried to reference the variable from outside of the if block, though:

<figure>
  <img src="/assets/images/LetVsConst.jpg">
  <figcaption>Transpilation when Referencing Let Outside of If Block</figcaption>
</figure>

Not only was the `let` transpiled to a `var`, **but the transpiled variable name was changed to `_a`**. Since the simple conversion of `let` to `var` would still make the variable accessible outside of the if block, Babel simulates the behavior of `let` by changing the name of the variable to make it inaccessible outside of the block. 

My `console.log` statement inside of the block was also changed to match the new variable name, since it was in the same block as the `let` statement and should have access to the variable.

The implementation here makes sense: **Babel emulates ES6 variable behaviors with ES5 syntax by modifying the name of variables defined with `let`.** This name modification only appears to happen, though, if an attempt is made to reference a variable outside of its block scope.  

Very cool!

[babel]: https://babeljs.io/repl/
