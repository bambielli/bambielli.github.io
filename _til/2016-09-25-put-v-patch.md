---
title:  "Put vs Patch, and Other Fun With HTTP Verbs"
date:   2016-09-25 22:30:00
category: til
tags: [http]
---

TIL the difference between PUT and PATCH, and found a nifty table highlighting

PUT is idempotent, while PATCH is not idempotent. What does idempotent mean, you might ask? It means if you do the same operation over and over again, you will get the same result.

This law does not apply to PATCH requests, since you are only partially updating a resource on the server... You can make the same request over and over again, then someone can come in with another PATCH for a different part of the resource you're modifying and suddenly your PATCH is no longer the same.

PUT requests, on the other hand, ARE idempotent, since every Put request is either updating or creating an entire object on the server. Meaning if you pass in partial data to a PUT request, that data will be the ONLY data available for the resource. This is idempotent, since we can always expect to be fully updating or creating a new resource on the server with the exact data we specify.

I was reading [this article][article]{:target="_blank"} which had the following awesome table that highlighted the different behaviors of HTTP verbs in a very clear way.

<figure>
  <img src="/images/HttpVerbTable.jpg">
  <figcaption>Frequently used HTTP verbs</figcaption>
</figure>

I would use PUT to perform updates of any kind, but now realize that doing PATCH requests when the server is partially updating an object better conforms with HTTP verb standards. Performing non idempotent behavior is not ideal (creates side effects). I guess if we want a user to be able to update an object on the server, we could replace the object with a full copy of that object server-side and save that as a new object. This operation is "pure" in the sense that every time we execute the update, the same expected outcome will be guaranteed.

This logic feels similar to reducers in Redux, which also expect immutable objects and state.

[article]: https://apihandyman.io/do-you-really-know-why-you-prefer-rest-over-rpc/
