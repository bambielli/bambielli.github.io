---
title:  "Mocking the Clock Using Jasmine"
date:   2016-03-02 14:56:00
category: til
tags: [testing, unit-test, jasmine, javascript]
---

TIL how to mock the internal clock of a test using jasmine. This was useful for testing methods that rely on a __timeout__ or __debounce__ before firing.

I recently wrote a method that is debounced by 500ms. Multiple calls to the method will reset the 500ms timer, and the method will only execute one time after the timer reaches 0.

Jasmine has a [nifty feature][nifty feature]{:target="_blank"} to test methods that are debounced or use a timeout: by mocking the internal clock of the test file, the tester can advance the clock manually trigger method calls and see how the method responds.

__Example:__

{%highlight javascript%}

describe('debounced test', function(){

	beforeEach(function(){
		jasmine.clock().install();
	});

	afterEach(function(){
		jasmine.clock().uninstall();
	});

	it('should call the method only after the 500ms debounce', function(){
		scope.debouncedMethod();

		jasmine.clock().tick(501); //increment clock 501 ms.

		scope.$digest();

		//assertions follow
	});
});

{%endhighlight%}

Nifty!

[nifty feature]: http://jasmine.github.io/2.0/introduction.html#section-Mocking_the_JavaScript_Timeout_Functions
