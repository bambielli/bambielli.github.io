---
title:  "What is a Breaking Change Anyways?"
date:   2018-01-12 21:49:00
category: til
tags: [api, service, contract, breaking, change, breaking change, signature, method, backwards, compatible, forwards, backwards compatible, forwards compatible, interface]
---

TIL what a breaking change was in the context of a public API or Service.

At work, I am currently working on the public API interface that will be exposed externally to our customers and partners. We are working with teams internally to get their resources ready to be exposed in this interface.

One of the things we have been impressing on our internal partner teams, is that once a resource is exposed on the interface, **breaking changes** will no longer be allowed without rolling the version of the client-facing interface.

We want our clients to be able to integrate with our platform with as little friction as possible. Ensuring our API teams internally are not pushing breaking changes to their APIs will lead to consistent contracts that are easy to integrate with.

But what constitutes a breaking change? And why are breaking changes so disruptive?

## What is a Breaking Change?

As an API or Service owner, **breaking changes are non-backwards compatible changes to the contracts of methods you expose to your consumers through your API interfaces.**

Breaking changes have the potential to literally "break" the applications that consumers of your API have built on top of the endpoints you have exposed. Client apps built on your APIs encode specifics of the `contracts` for each endpoint in their apps, so they can make requests for data they are interested in.

For example, the contract of an endpoint to get a list of dogs from an API might look as follows:

```
Endpoint:
GET /v1/dogs

Required Headers:
- x-api-key
- Authorization

Response:
[
  {
    id: integer,
    name: string,
    age: integer,
    breed: string
  },
  ...
]
```

The above defines the contract for the `dogs` endpoint. Enumerated, the various pieces of the contract are as follows:

  1) The URI to access the resource (/v1/dogs)
  2) The protocol / method used to interact with the endpoint (HTTP / GET)
  3) Request parameters necessary to access the resource (api-key and authorization headers)
  4) The response model for the endpoint (in this case, an array of objects with the specified keys + types)

If non-backwards compatible changes are made to these contracts, clients will break, and you will have unhappy customers.

This doesn't mean that changes can't be made to the APIs and services themselves! **As long as the client-facing contract remains intact, changes can be made without worry of it being a breaking change**. This can often be accomplished with the addition of an [adapter][adapter] that takes APIs with new interfaces and converts them to the publicly exposed one.

## Backwards Compatibility

We've defined that a breaking change is a non-backwards compatible change to a publicly exposed contract, but what constitutes a backwards compatible change?

**Any change that wouldn't cause clients to break would be considered a backwards compatible.**

That's kind of a meta definition... so here are some **examples of backwards compatible changes** to our dog resource above:

1) **Adding new fields to the response model** - If i added a `vaccinations key` to my response model, that wouldn't break my consumers! Even though the new key is sent in requests for the dog resource, the presence of that key won't cause client code to stop working... since clients weren't aware of it until I added it, they will just continue to ignore it in their code until they upgrade.

2) **Adding new methods to the interface** - If I added a `/v1/cats` resource to my interface, that won't cause clients to break! They didn't know about it before, and its presence on the interface shouldn't affect their retrieval of Dogs.

3) **Adding new optional request parameters** - Say I wanted to allow my clients to filter by breed, so I added a query parameter for breed to my dog resource (e.g. `/v1/dogs?=cavalier`). This also shouldn't break clients, since the response from the original request for `/v1/dogs` remains unchanged.

Astute readers will have noticed something about the above three items... they all start with `Adding`. **Adding things to your interface is generally a backwards compatible change.** Non backwards compatible changes all revolve around Updating, or Deleting properties of your interface contracts.

**Non-backwards compatible changes** to the dog resource include:

1) **Changing the names of fields in the response model** - If the `breed` key was renamed to `Breed`, clients looking for the breed of dogs in the response model wouldn't be able to find the value anymore until they refactored to include capitalization of the key name.

2) **Deleting a field from the response model** - If the `name` key was deleted entirely... that is clearly not backwards compatible and would break all consumers looking to read names of dogs out of the response model.

3) **Changing the shape of the response model** - If the response object became nested that would also break consumers.

4) **Changing the URI of the resource** - If the URI changes, clients won't be able to find your resource anymore. Definitely a breaking change, and not backwards compatible.

5) **Changing the method used to access a resource** - If I change access of the `/v1/dogs` resource from a GET to a POST for some reason, that will break my consumers.

6) **Adding new required request parameters** - If I need the users to start passing multiple API keys, or if I need them to start passing the version of the resource they want to access as a header, that's breaking.

So clearly there are many more ways to make changes that break consumers than otherwise. Some breaking changes are so subtle that it is difficult to pick up on them unless you are constantly vigilant.

Requiring API owners to not make breaking changes to their interfaces imposes more rigidity on how services evolve. Some would argue that to make all changes backwards compatible, interfaces become large and clunky.

I think the decoupling provided by an adapter layer that performs conversions to the expected interface contract is ultra- valuable here, and worth the investment up front (even if it is just pass through).

The point here is that we want to make integrating with our public API interface as frictionless as possible for our customers.

We want more customers to build on our platform. If our platform is constantly breaking, the client-rework needed to conform to an ever-changing interface might outweigh the value provided by accessing the underlying resources.

There are certainly other breaking / non-backwards compatible changes to APIs that I didn't mention, so feel free to share in the comments!

[adapter]: /posts/2017-09-09-the-adapter-pattern/
