---
title:  "Native vs 3rd Party Promise Implementations"
date:   2016-12-04 09:30:00
category: til
tags: [promise, es6, bluebird]
---

TIL that the promise implementation shipped with ES6 is not very performant, and that it is still preferable to use a 3rd party promise library for several reasons.

Note that this post will focus on the 3rd party promise library [Bluebird][bluebird]{:target="_blank"}

### Why not Native?

I understood that pre-ES6 when there was no native promise implementation, 3rd party libraries were written to fill that gap. Now that native promises are available for use, why use a library to polyfill the behavior?

It appears that there are a few good reasons.

### More Performant?

I stumbled across [this][so]{:target="_blank"} StackExchange post which asks why native ES6 promises appear slower and less memory efficient than Bluebird promises. It points to a perf test that shows **Bluebird promises are around 4x faster and use 4x less memory than native ES6 promises to resolve.**

Farther down in that post the Bluebird author responds with some reasoning for the performance differences. The post was written in April of 2015, and after some additional searching I couldn't tell if the performance data being referenced was still accurate. The performance test data referenced in the README of the bluebird repo was last updated in January of 2015.

I poked around a bit in the bluebird source and the author made it quite easy to re-run benchmarking. Check out the data below. I ran from the [following branch][branch]{:target="_blank"} which contains a fix for upgrading to the latest version of the "streamline" library (another 3rd party async javascript lib).

```
Dependency versions
├── async@1.5.2
├── bluebird@3.4.6
├── davy@1.3.0
├── deferred@0.7.5
├── kew@0.7.0
├── lie@3.1.0
├── optimist@0.6.1
├── promise@7.1.1
├── q@1.4.1
├── rsvp@3.3.3
├── streamline@2.0.13
├── vow@0.4.13
└── when@3.7.7
├── rx@2.5.3
├── co@4.6.0
├── baconjs@0.7.89
├── highland@2.10.1
```

**Benchmarking sequential (./bench doxbee)**

```
results for 10000 parallel executions, 1 ms per I/O op

file                                       time(ms)  memory(MB)
callbacks-baseline.js                           118       30.55
callbacks-suguru03-neo-async-waterfall.js       174       43.49
promises-bluebird-generator.js                  213       38.16
promises-bluebird.js                            261       48.12
promises-cujojs-when.js                         357       61.63
promises-then-promise.js                        366       57.88
promises-lvivski-davy.js                        445       91.46
promises-tildeio-rsvp.js                        505       86.60
callbacks-caolan-async-waterfall.js             617       98.84
promises-dfilatov-vow.js                        671      147.82
promises-calvinmetcalf-lie.js                   828      138.86
promises-ecmascript6-native.js                 1123      183.64
generators-tj-co.js                            1170      137.32
promises-obvious-kew.js                        1215      214.58
promises-medikoo-deferred.js                   1841      164.09
observables-Reactive-Extensions-RxJS.js        2163      224.04
streamline-generators.js                       2262      180.45
streamline-callbacks.js                        3584      173.93
promises-kriskowal-q.js                       11226      835.02
observables-baconjs-bacon.js.js               34624      808.23
observables-pozadi-kefir.js                   56509      145.96
observables-caolan-highland.js               128962      457.45

Platform info:
Darwin 16.0.0 x64
Node.JS 6.7.0
V8 5.1.281.83
Intel(R) Core(TM) i5-5257U CPU @ 2.70GHz × 4
```

**Benchmarking parallel (./bench parallel) --p 25**

```
results for 10000 parallel executions, 1 ms per I/O op

file                                      time(ms)  memory(MB)
callbacks-baseline.js                          268       64.71
promises-bluebird.js                           425       99.67
callbacks-suguru03-neo-async-parallel.js       431       82.30
promises-bluebird-generator.js                 494      103.07
promises-lvivski-davy.js                       740      157.46
promises-cujojs-when.js                        935      162.30
callbacks-caolan-async-parallel.js             943      147.88
promises-then-promise.js                      1408      312.29
promises-tildeio-rsvp.js                      1613      350.70
promises-calvinmetcalf-lie.js                 1745      369.84
promises-ecmascript6-native.js                2379      448.05
promises-dfilatov-vow.js                      2850      528.65
promises-medikoo-deferred.js                  4588      413.72
promises-obvious-kew.js                       5024      634.53
streamline-generators.js                     23243     1201.41
streamline-callbacks.js                      43371     1312.59

Platform info:
Darwin 16.0.0 x64
Node.JS 6.7.0
V8 5.1.281.83
Intel(R) Core(TM) i5-5257U CPU @ 2.70GHz × 4
```

Looks like Bluebird still comes out on top for both speed and memory usage when compared to other promise implementations.

When compared to ES6, it is still **4.3x faster and 3.8x more memory efficient**, at least under these test conditions.

Impressive!

### Killer Feature: Promisify

Performance aside, Bluebird offers an awesome feature called [promisification][promisification]{:target="_blank"}, which allows you to wrap another library and have it return promises instead! Examples from the docs:

```
var fs = require("fs");
Promise.promisifyAll(fs);
// Now you can use fs as if it was designed to use bluebird promises from the beginning

fs.readFileAsync("file.js", "utf8").then(...)
```

You can do the same thing with database clients, or other async libraries that are not natively promise-based (and instead accept callbacks).

Also amazing!

### Can I use?

According to [can I use][can]{:target="_blank"} native promises are ready for use in most current version browsers today, but not all. If you're looking to support legacy browsers (or IE), using a 3rd party promise library is a nice alternative to relying on an implementation in the browser.

### Conclusion

It seems like of your 3rd party async-handling Javascript libraries, bluebird is still the best bet. It offers promisification and is more performant than native ES6 promises. I'll be using it in my next project for sure.

[bluebird]: http://bluebirdjs.com/docs/getting-started.html
[so]: http://softwareengineering.stackexchange.com/questions/278778/why-are-native-es6-promises-slower-and-more-memory-intensive-than-bluebird
[branch]: https://github.com/bjouhier/bluebird/tree/fix-streamline-benchmark
[promisification]: http://bluebirdjs.com/docs/api/promisification.html
[can]: http://caniuse.com/#feat=promises
