---
layout: post
title:  "Mocking Angular Dependencies in Unit Tests"
date:   2016-02-29 15:12:00
category: til
tags: [angular, testing, unit-test, jasmine]
---

TIL how to properly mock angular dependencies in Unit Tests using the Jasmine test framework.

[A wise man once said...][wise]{:target="_blank"}

> Angular was written with testability in mind

This was something I have kept in the back of my head since I started working with Angular a little over a year ago, and have honestly struggled to understand... and today I was finally struck with clarity: __angular is incredibly easy to unit test__, thanks to [`dependency injection`][dependency]{:target="_blank"} and the ease with which dependencies can be `mocked`.

I won't go in depth in to `dependency injection` here (maybe another day...) but the fact that angular leverages this pattern throughout construction of its controllers and services makes mocking and testing units of code a breeze.

If a unit under test relies on a method call to an injected dependency, the [jasmine test framework][jasmine]{:target="_blank"} makes it easy to mock and call fake methods whenever the dependency is needed, allowing the tester to focus on the unit under test instead of first making sure a particular dependency is functioning as expected.

Note: testing with realistic responses from the backend should be reserved for **integration** tests, and should not be of concern at the *unit* level.

__Example:__

I just spent the last week testing changes to a controller that leverages a service to request data from a paged API endpoint for our app. The controller  makes a call to the service on initial load to request a default set of data.

Initially, my tests all looked something like this:

{%highlight javascript%}

describe('test $scope.manipulateMembers', function(){
	beforeEach(function(){
		var data = load(data_from_fixture); //takes a long time
		$httpBackend.expectGET('default/api/request').respond(200, data);
	});
	it('should update $scope.loaded to be true', function(){
		$rootScope.$digest();
		$httpBackend.flush(); //takes a while
		//assertions begin here
		...
	});
});


{%endhighlight%}

Each describe block started with an expectation for a request to the backend for the default set of data. While this is good to check in an *integration* scenario, __the interaction between the backend and the controller should be irrelevant when testing a unit of code in the controller.__ The unit should function the same whether the backend request is made or not.

Furthermore, the data we were returning each time was quite large (to emulate a realistic set of data being returned from the backend) and was re-loaded from a fixture before each test. __I was performing an integration test with the backend for each of my unit tests.__

Additionally, asserting that the backend was called correctly when the controller was loaded and flushing the backend for each unit added a TON of time to each test: __tests took upwards of 2000ms a piece__, and often would cause our karma server to disconnect in the middle of a test run because tests were running so slow.

Luckily, I realized this didn't need to be the case, as I figured out how to mock the default call to the service that interfaced with the backend by using jasmine `spys` and `mocks`.

{%highlight javascript%}
describe('test $scope.manipulateMembers', function(){

	beforeEach(function(){
		spyOn(serviceName, 'serviceMethod').and.callFake(function(){
			var deferred = $q.defer();
			deferred.resolve(['some', 'fake', 'data']);
			return deferred.promise;
		});
	});
	it('should update $scope.loaded to be true', function(){
		$scope.$digest();
		expect($scope.loaded).toBe(true);
	});
});

{%endhighlight%}

By ___mocking___ the default call to the backend service via a jasmine spy, we can forgo dealing with the backend request expectations + loading realistic data from a fixture on initial digest.

__Test execution time was reduced from over 2000ms per test, to 10ms per test.__

This type of mocking is possible with __ANY__ service that may be necessary for testing a particular unit of code in my controller.

This was a truly enlightening experience overall, and really helped solidify my understand of that timeless phrase **angular was written with testability in mind** as well as the differences between `unit` and `integration` tests on the frontEnd.

I hope you feel enlightened as well.

Happy Leap Day!

[wise]: https://docs.angularjs.org/guide/unit-testing#with-great-power-comes-great-responsibility
[dependency]: https://docs.angularjs.org/guide/di
[jasmine]: http://jasmine.github.io/2.0/introduction.html#section-Spies
