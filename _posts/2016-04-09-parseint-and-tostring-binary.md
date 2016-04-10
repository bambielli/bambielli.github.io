---
layout: post
title:  "Converting Binary Strings to Different Bases"
date:   2016-04-09 08:20:00
category: til
tags: [javascript]
---

TIL how to convert a binary string to a digit of any base, and how to convert a digit of any base to its corresponding binary string.

This weekend was the annual [Google Code Jam][gcj]{:target="_blank"} which I try to participate in every year. I don't know if the questions this year were easier, or if I am just a better programmer, but I was able to submit answers for 3 of the 4 qualifying problems and qualify for the next round. Although I could have written my answers in Python, I decided to code in Javascript as I have been practicing with it quite a bit lately.

### JamCoins Problem

The third question in the qualification round involved coming up with all possible permutations of a binary string of N bits, the first and last of which were required to be '1'. The success condition involved testing the binary strings constructed above to see if their values converted to base 2 through 10 were NOT prime. I needed a way to iterate through all binary string permutations with the constraints above, and then convert those binary strings to their equivalent values in bases 2-10.

I started by trying to think of an algorithm that, given an array of the inner '0's for the binary string, would return all possible permutations of those bits and then push/shift '1's to the ends. This would have involved storing all permutations in memory, which sucks because for the large problem we were working with 32 bits. We also didn't necessarily have to examine ALL permutations of the N bits, because the problem's solution condition could be potentially satisfied with N-J bits.

### The Chosen Implementation

My chosen implementation only required storing the current binary string in memory, testing it against the problem solution condition, and then iterating to the next. I created a for loop that would do this for me. See code below for explanation:

{%highlight javascript%}

var startBinaryVal = Math.pow(2,N-1) + 1; //starting value since binary string always has 1 at both ends;
	var max = Math.pow(2, N) - 1; //maximum value obtainable by N bits
	for(value = startJamVal; value <= max; value += 2) {
		var binaryValue = value.toString(2);

		for(var i = 2; i < 11; i++) {
			var parsedValue = parseInt(binaryValue, i);
			//test against other success criteria
		}

	}

{%endhighlight%}

My loop starts at the decimal value for N binary digits where the first and last bits are '1's. This is calculated by `2^(N-1) + 1`. The loop increments by 2 each time, to ensure that the value always has a 1 at the end, and stops when the value exceeds the maximum value that N binary digits can possibly hold, which is `2^N - 1`.

Inside of the loop I use `value.toString(2)` to get the equivalent binary string representation of the decimal value from the loop. By pasing a numerical parameter to the `Number.toString()` method, ***[toString()][toString]{:target="_blank"} will return the converted string value in that base.***

I then take this binaryString, and parse an integer of base i out of it. ***[parseInt()][parse]{:target="_blank"} accepts a string and a base as its parameters, and converts that string to the chosen base.***

I always knew about `parseInt()` and `Number.toString()` but hadn't used them in this context before. I feel like I can convert between anything, now!

I qualified for the next round of the GCJ, so I'll update here with any more learnings / thoughts from solving those problems.

[gcj]: https://code.google.com/codejam
[parse]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt
[toString]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toString
