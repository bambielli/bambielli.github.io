---
title:  "SOLID: Dependency Inversion Principle"
date:   2017-12-29 13:19:00
category: til
tags: ["dependency", "inversion", "principle", "dependency inversion principle", "dependency-inversion-principle", "architecture", "design", "software"]
---

TIL about the dependency inversion principle, and how it (like most of the other SOLID principles) promotes loose coupling between modules.

The `Dependency Inversion Principle` is the `D` in SOLI**D**, and is yet another way to design loosely coupled modules in an application.

The dependency inversion principle states that:

```
A. High-level modules should not depend on low-level modules. Both should depend on abstractions.
B. Abstractions should not depend on details. Details should depend on abstractions.
```

I used this principle recently at work without even realizing it.

We are using a logging service called [`sentry`][sentry]{:target="_blank"} to capture client-side errors in our application.

Sentry provides a package called `Raven` that makes connecting to sentry and pushing logs a breeze.

One option we had when integrating sentry in our app was to access Raven directly from relevant modules, pushing errors as they occur in high level business logic / view layers. **This would have resulted in tight coupling between our modules and the Raven service.** If we decided to use another logging service, we would need to change and re-test many modules in our app in order to ensure the new service was integrated appropriately.

Instead, we created a `client-logger` interface, which exposes a few methods that in turn make calls to the relevant `Raven` methods. Modules in our code access logging through the `client-logger` interface instead of accessing Raven directly, which promotes loose coupling between the two modules. Swapping out logging services becomes a much smaller chore now, since all of the logic for configuration and usage of Raven is encapsulated in the client-logger interface.

This is, in essence, what the dependency inversion principle calls for. By promoting loose coupling between high level and low level modules, and "inverting" the dependency on the interface from high level (business logic layer) to low level (logging layer), we minimize the impact of changes in the low level components from the high level (high value) portions of our app.

[sentry]: https://sentry.io/welcome/
