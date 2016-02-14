---
layout: post
title:  "Global Variables in Javascript"
date:   2016-02-11 18:22:00
category: til
tags: [javascript]
---

TIL that global variables are automatically (magically?) set on *window* in javascript.

	<script>
	    global_id = 4;
	</script>

Is equivalent to:

	<script>
	    window.global_id = 4;
	</script>

I also learned that the javascript "this" keyword, when used outside of a closure, also refers to *window*.