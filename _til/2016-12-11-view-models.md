---
title:  "The Benefits of View Models"
date:   2016-12-11 11:34:00
category: til
tags: [view-model, architecture, api, java]
---

TIL that view models have utility beyond making data easier to digest by consumers.

## Beyond Restructuring

Prior to this revelation, view models to me were just a way to re-format data in an endpoint such that the end structure would be easier to consume. To state differently: **View models are a different representation of your data than how it is persisted to the database** that are constructed for convenience purposes or because consumers require data in a specific format.

This is not the only reason for the creation of view models.

## View Models Help Define Your API Contract

View models are, in effect, **the contract between you and your consumers for how data shall be structured in a response.** When you define a view model, you are defining how consumers should expect the data they request to be returned.

The request parameters (request contract) and view model (response contract) make up the full contract for an endpoint. Once your API is in production, neither should be changed without notifying consumers since changes to contracts can lead to breaking changes in consuming applications.

If your API does not return view models and instead returns database entities directly, **your database schema becomes part of your API contract.** Therefore, if you make a schema change that affects the corresponding entity class in your API you could be introducing breaking changes to your consumers.

**View models decouple the data being returned by a view from database entity design.** If your endpoint returns a view model you can make changes to the underlying database schema, create a mapping between database entity and view model, and  guarantee no breaking changes for your consumers.

## Applying this Knowledge

Currently we have an API that defines its view models using the [JsonSchema2Pojo][pojo]{:target="_blank"} generator. JsonSchema2Pojo takes a [json schema][json]{:target="_blank"}, and generates a pojo with builder methods, json serialization, and setters and getters. Using a generator like this for view model classes prevents silly errors that could occur when writing them manually, and also allows you to **share view model json schemas with consumers** so they can easily generate their own copies of the API return types.

A few of our endpoints currently return entities directly, because the format of the entity is the format we wish to send to consumers. We are planning on creating view-model intermediaries for these that are essentially the same class generated via a json schema. There are a few reasons for this:

1) We will now have a json schema to share with consumers so they can auto-generate their own view-model classes as well.

2) Surely the entity design will need to change in the future and its structure will deviate from how we want to ship data to requestors; the view model provides the guarantee that as long as data conforms to its structure, consumers will not experience breaking changes.

I've learned a lot about API design over the past week, and I've also learned about a few other Java dependencies that make quick API construction a breeze, including [Retrofit][retro]{:target="_blank"} by Square, the [Hibernate][hibernate]{:target="_blank"} ORM, and some of the Spring dependencies for interacting with various databases like the [spring-data-redis][spring-data-redis]{:target="_blank"} project. Look out for follow up blog posts about these in the future.


[json]: http://json-schema.org/
[pojo]: http://www.jsonschema2pojo.org/
[retro]: https://square.github.io/retrofit/
[hibernate]: http://hibernate.org/
[spring-data-redis]: http://projects.spring.io/spring-data-redis/
