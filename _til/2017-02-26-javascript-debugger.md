---
title:  "Javascript Debugger"
date:   2017-02-26 14:28:00
category: til
tags: [debugging, javascript]
---

TIL of two ways to work debug statements into javascript code. 

I always knew that you could open the sources tab in your browser's dev tools, and add a breakpoint by clicking on the line you want to break on, then refreshing the page. What I didn't realize, though, was that you could add breakpoints in your code manually with the `debugger` statement.

### Debugger statement

Apparently this has been around since at least 2012 (that was the earliest blog post I could find that referenced it) but I just learned about it recently. 

By adding a `debugger` statement to your code, your script will pause at exactly that point when it is initially loaded in the browser. 

{%highlight javascript%}

var name = 'Brian';
debugger; //script will pause here when executed in the browser.

function modifyName() {
  name = 'Alex';
}

modifyName();

{%endhighlight%}

This is a nice alternative to inserting breakpoints via the "sources" tab in the dev tools, since you can add `debugger` statements before initially loading the page.

### Sometimes It Doesn't Work?

I found when I was initially playing around with the `debugger` statement in my code, that sometimes it wouldn't take effect.

I eventually found that for the `debugger` statement to pause execution of your script, *you need to have your devTools open*. Otherwise the browser will skip the debugger statement. 

I guess this is a good thing, in case you accidentally leave a debugger statement in your code when you push to prod. Still, if using debuggers in your code, it is probably best to include some sort of linting rule or step in your code review process to confirm no debuggers are left in scripts before merging.


