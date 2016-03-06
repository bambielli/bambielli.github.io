---
layout: post
title:  "Overwriting Mocks in Tests"
date:   2016-03-04 11:30:00
category: til
tags: [angular, testing, unit-test, jasmine]
---

TIL how to overwrite mocks in tests, by saving them to a variable and modifying the function tied to the object.

During my *test-a-palooza* (technical term) over the past few days, I have been learning a lot of advanced jasmine techniques.

In my test file, I found that I wanted to overwrite a spy that I had previously created in a `beforeEach`. Here is an example of the spy:

{%highlight javascript%}
beforeEach(function(){
   spyOn(serviceName, 'serviceMethod').and.callFake(function(){
      var deferred = $q.defer();
      deferred.resolve(mock_data);
      return deferred.promise;
   });
});
{%endhighlight%}

The spy above is `overriding` the method call to the service that interfaces with our application backend to fetch data. It is returning a __resolved promise__ with mock_data, which imitates a *successful* HTTP request...

...but now I need to test how my controller handles an *unsuccessful* response from the backend! If I try to create a new spy in my method, Jasmine throws an error saying `spy on serviceName, serviceMethod already exists`.

__The solution:__

The `spyOn` method returns the created spy as an object when it is invoked. If you just save that object to a variable, you can delete it as necessary and re-create it to return a rejected promise in my edge case test!

__Even Better:__

Instead of deleting the spy completely, it is possible to just override the method *again* with a new `callFake`!

{%highlight javascript%}
beforeEach(function(){
   // create a variable to store the spy
   var serviceSpy = spyOn(serviceName, 'serviceMethod').and.callFake(function(){
      var deferred = $q.defer();
      deferred.resolve(mock_data); // resolved promise
      return deferred.promise;
   });

   it('should handle a rejected promise appropriately', function(){
      // use the variable to overwrite the fake method in this test ONLY
      serviceSpy.and.callFake(function(){
         var deferred = $q.defer();
         deferred.reject({message: 'error'}); // rejected promise
         return deferred.promise;
      });

      //assertions here
   });
});
{%endhighlight%}

__Furthermore__, if a spy already exists for a dependency but you need to call the actual implementation of that dependency again temporarily for a test, you can use `spy.and.callThrough()` to overwrite the fake method with the original implementation

Spys rock, and make testing SO much easier than it ever has been. I thought I had a good grasp on spys and when to use them, ___but now I feel like agent M deploying 007 all OVER these controllers and services.___

get it? __spys???__

(badjokeeel)
