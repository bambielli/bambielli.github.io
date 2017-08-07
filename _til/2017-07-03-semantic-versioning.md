---
title:  "Semantic Versioning"
date:   2017-07-03 19:38:00
category: til
tags: [architecture, versioning]
---

TIL what semantic versioning is, what each number of the version indicates, and why using a versioning scheme like semantic versioning is important.

## What is Semantic Versioning

[Semantic Versioning][semver]{:target="_blank"} (also referred to as SemVer) is a set of rules proposed for assigning a version number to your software. Under semantic versioning, each number assigned to a version conveys something meaningful about the changeset that occurred between two versions.

## MAJOR.MINOR.PATCH

There are **3 numbers** assigned to each version of a semantically versioned piece of software. Each number indicates something about the changeset applied at that version. The numbers are generally separated by periods, and are written in the format `MAJOR.MINOR.PATCH`.

- **MAJOR** - This number indicates that **breaking changes** have occurred in the software package. If a consumer of your software is going to perform a MAJOR version upgrade, they can **expect to make fixes throughout their application** to conform to the requirements of the new interface introduced by the MAJOR version. Breaking changes include the deletion of fields in response models, or the renaming of URIs exposed in an API.

- **MINOR** - An incremented MINOR version means that **backwards compatible** (non-breaking) **additions** have been made to your software. Consumers can expect that upgrading will not result in their application breaking, but will instead **expose new functionality** that they can incorporate in to their product later. Backwards Compatible additions include the addition of new fields to a response model, or the addition of new URIs to an API. Consumers can choose to incorporate this new functionality now, or wait until they have bandwidth to do so, but either way **they should be able to upgrade without downtime** when a MINOR version changes.

- **PATCH** - A PATCH version indicates that backwards compatible bug fixes were made to the current version of the software. These are generally the safest upgrades that consumers can make, since there is no new functionality introduced and no new breaking changes. Examples of PATCH type changes include performance enhancements.

## Why Should I Follow SemVer?

It's in the name! **Semantic** versions convey so much more information to consumers of your software than just any old number could. By following the rules outlined above, **consumers will have confidence when upgrading to newer versions of your software, that the changes made between versions will not break their products.**

Giving simple version numbers this much power feels great, but it's still up to the developers of published software packages to ensure that breaking changes are not slipped in to MINOR or PATCH versions. **Make sure everyone on your team is aware of what constitutes a breaking change.** For new developers, and even seasoned devs who have never worked on publically accessible products before, it's worth getting on the same page.

A versioning scheme like semantic versioning can be particularly useful when developing fine grained microservice systems, to ensure changes to services are well communicated, and well thought out, before they are published to the rest of the team to use. The **well thought out** piece is especially relevant: since MAJOR versions are clearly the most disruptive, developers will naturally think twice before making a change that constitutes a MAJOR change.

There's a level of **trust** that goes along with semantic versioning to ensure you don't accidentally break your consumers. Your consumers are your users, and you should treat them right! A team that uses semantic versioning well will definitely have happy consumers.

[semver]: http://semver.org/