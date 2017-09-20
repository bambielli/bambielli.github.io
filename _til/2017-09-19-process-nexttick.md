---
title:  "Using process.nextTick to test asynchronous actions"
date:   2017-09-19 21:58:00
category: til
tags: [javascript, testing, test, jest, react, node]
---

TIL how to use `process.nextTick` to ensure asynchronous actions are resolved before running assertions during tests.

## Testing a Client Side Model

Today I was trying to write a test for one of our client side model objects. The model has a static method "getModel", which fetches relevant data from the server and returns a model object representation of the data.

We use the library `nock` to mock network requests, so mocking the fetch call was simple. The difficult part was that **the getModel method involves additional processing of the response from fetch** in a `Promise.then`.

I found that my assertions on the model object returned from the `getModel` call were being run before this additional processing had completed.

## The solution

A cheap way of getting around this issue might be to introduce an **explicit wait** in the tests, using `setTimeout`. This would guarantee that all promises have resolved before making assertions on the return object.

Introducing explicit waits in tests, though, is bad practice. It will eventually lead to flakiness in your tests (i.e. false negatives) and will slow test execution down.

Instead, I learned about `process.nextTick(callback...)`, which is a `node` utility that adds the passed callback to the nextTick queue. The nextTick queue is run **after all events in the current eventLoop have finished**.

By wrapping my assertions inside of a `process.nextTick`, I was able to guarantee without the use of explicit waits or timeouts that all promises initiated by my test had completed processing.

According to the [node documentation][node]{:target="_blank"}: __process.nextTick is not a simple alias to setTimeout(fn, 0). It is much more efficient. It runs before any additional I/O events (including timers) fire in subsequent ticks of the event loop.__

[node]: https://nodejs.org/api/process.html#process_process_nexttick_callback_args