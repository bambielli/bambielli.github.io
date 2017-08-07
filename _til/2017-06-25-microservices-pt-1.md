---
title:  "Microservices pt. 1: Defining Microservices"
date:   2017-06-25 12:20:00
category: til
tags: [architecture, microservices, system, design]
---

TIL what a microservice is, what the purported benefits of microservices are, and some of the drawbacks of creating a system built from microservices.

I've been reading [Building Microservices][micro]{:target="_blank"} by Sam Newman of Thoughtworks origin. Newman's book is insightful and clearly written, covering both the benefits and drawbacks of microservice system design.

I'm planning on writing a few posts as I draw insight from the book.

## What is a Microservice?

A `microservice` helps us represent the real world in our code.

By breaking down a small piece of our domain (often referred to as a [`bounded context`][martin]{:target="_blank"}) into a standalone service that is purely focused on operating on that single piece, we strive for `strong cohesion` of code in the service and `loose coupling` between different services.

**How small is too small?** Newman argues that services should be "small enough and no smaller"... a somewhat flippant answer, but he argues that seasoned developers can tell when something feels "too big" and when something feels "small enough". The definition of "too big" tends to change from team to team and domain to domain, so having a consistent answer for every case doesn't seem to be practical.

Loose coupling between services is critical to a well-functioning microservice system, but it's harder than it sounds. In my experience with microservices, it is easy to get in to an integration mess where services depend on each other either through API interfaces or internal models.

The key question you should ask when developing your system: **"Can you deploy changes to a service without changing anything else in the system?"** If not, your services have an undesirable amount of coupling, and you should spend time re-thinking your service boundaries and design.

## Benefits of Microservices

There are a few key benefits that microservices offer when compared to their `monolith` counterparts.

1. **Technological Heterogeneity** - Since services are deployed independently and are not coupled to each other, we have more freedom in our technology choices for each service. If data for one service is best modeled as a graph, great, use a graph DB like [Neo4j][neo4j]{:target="_blank"} for it! If another is better serviced by a relational data store like Postgres or MySql, fantastic, Use it! Additionally, trying out new technologies becomes less risky with microservices: I can isolate an experiment to one service instead of putting the entire system at risk. Picking the right tech for the job is a great benefit of microservices, but it does present the risk of requiring a team of developers well versed in many languages and tech stacks to service the system in the long term.

2. **Resilience** - With microservices, failures are now isolated to a smaller chunk of our system. We also introduce new sources of failure that monoliths don't need to deal with, like the inevitability of network failures. The challenge then becomes, how do we architect our system of services to gracefully degrade service to users of our system when a service goes down?

3. **Scaling** - Scaling a monolith is costly: if one portion of your system is under heavy load, the entire system, including those parts not being used, will scale with it as new instances are spun up. Not so with microservices! If one service is under load, that service alone will scale, leaving the rest of your system at levels appropriate for the work they need to do. Deploying microservices on platforms with "on-demand provisioning" capabilities like AWS or Heroku, means we can keep the costs of running our system low while guaranteeing high levels of performance for our users.

4. **Ease of Deployment** - Another impact of designing small and loosely coupled services is that deploying becomes a breeze. Contrast this to deploying a change to a monolith system, where the cost of deploy is so high that the organization maintaining it can sometimes freeze in response. Microservices make your systems and teams more agile, and increase the speed with which your team can respond to change.

5. **Organizational Alignment** - We all know that small teams working on small codebases tend to be more productive than large distributed teams working on monoliths. Microservices allow you to align your teams to different service boundaries: this can include geographical alignment, allowing co-located teams to be responsible for small chunks of the overall system.

6. **Composability** - In a world where systems serve many clients, microservices allow us to be flexible in how we reuse functionality to provide the best interfaces for our consumers. We can build different interfaces for mobile, tablet, desktop, wearable... all exposing a different API from our service boundary.

## A Warning on Going 'micro' From the Start

I've worked on a project that attempted to go micro from the project's inception... unfortunately this wound up being an enormous headache!

The engineers on the team were all new to the domain, and because of this **it was difficult to know where to draw the appropriate service boundaries.**

Because of this, service interfaces and models were **constantly changing**. There were frequent "reshuffles" of boundaries, and the integration of the services were all bound together through service clients with a strict versioning scheme that made deploying changes quite difficult.

After a few months of attempting a microservice architecture, we decided to bring our system together in to a monolith.

This is a cautionary tale: microservices have many purported benefits as I've described above, but getting service boundaries right is a critical step. **It is probably better for new teams who are new to a domain to initially go for a monolith approach**, then decompose later once they have more confidence that the boundaries they are drawing are correct for the domain.

I will cover some decomposition and modeling techniques in my next post.

[micro]: https://www.amazon.com/Building-Microservices-Designing-Fine-Grained-Systems/dp/1491950358
[neo4j]: https://neo4j.com/
[martin]: https://martinfowler.com/bliki/BoundedContext.html