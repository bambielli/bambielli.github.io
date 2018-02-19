---
title:  "Why Product Roadmaps are Important"
date:   2018-02-18 17:00:00
category: til
tags: [roadmap, software development, process, product, jira, estimation, agile, aha, planning, internal stakeholder, external stakeholder, prioritization]
---

TIL why product roadmaps are important: they provide necessary clarity to internal and external stakeholders, for the coordination and prioritization of medium-term features.

## What is a Roadmap

A [product roadmap][roadmap]{:target="_blank"} is a way to communicate high level product initiatives to internal and external stakeholders for the product.

`Internal stakeholders` refers to those directly working on the product, while `external stakeholders` include other teams that depend on the product, or teams that are otherwise interested in product status (like sales and marketing).

The roadmap reflects a **best guess at prioritization for medium-term product features** based on current knowledge of customer needs. The roadmap should be a living document that changes as new business priorities are discovered, and development team members should be organized in a way that allows them to pivot in response to these new learnings.

## How Does a Roadmap Benefit Internal Stakeholders?

A roadmap provides internal stakeholders with clarity on what comes next.

**From an engineering perspective**, a well defined roadmap promotes better technical decisions in the short-term. If medium-term features are known, engineering can start thinking about how today's design decisions might impact the implementation of the next feature that comes their way.

**From a product perspective**, a roadmap provides opportunity for buy in and discussion around the initiatives planned for the medium-term. Buy in is critical, since it gets everyone on the team involved and allows their opinions to be heard: this way, everyone knows what they are signing up for! This should give product more confidence that the scoped work is achievable when they report status back to external stakeholders. Using a tool like [Aha][aha]{:target="_blank"} provides a single source of truth for roadmaps across the business, and can help when collaborating with other team members on a roadmap.

An example of how roadmap buy-in / validation is useful: our product team was prioritizing the onboarding of several APIs to the uptake [developer portal][dp]{:target="_blank"}, with a goal of getting them onboarded by the end of Q1. Engineering knew of some complications with that ask, and we were able to work together to properly scope the Q1 outcome to something we knew was attainable in that time. We were also able to inject some engineering priorities in to the Q1 goals, based on technical debt / infrastructure work we knew needed to be done to support a higher volume of APIs on the portal. Being able to do this scoping and validation work in the context of the roadmap ensures that priorities are clearly communicated and bought in by all team members **before** dates are communicated to the business.

## External Benefits?

**From an external perspective**, the roadmap allows those with stake in the short-term outcomes of the product to peek in to the development machine and weigh in with their own priorities. For example, if there is a team that could leverage a few key features in the short term, it might make sense to re-prioritize those features in the absence of other product-driven requirements.

A concrete example of this for the developer portal team at Uptake: we were working towards some internal milestones, when we were approached by another team who was looking for a way to surface documentation about their APIs to a customer within the next month. We re-prioritized our short term goals to accomodate this ask, since the prospect of having invested users of our product (both internal to the company and external) was too valuable to pass up. This shift in priorities wound up benefiting both teams; a win-win for the business.

## "Check the Roadmap" - Protecting the Team

Imagine your team is in the middle of a series of tight deadlines, when suddenly a new team jumps in and starts asking for features that will benefit their team in the short term. “We really need feature X done ASAP, our customers are asking for it!”

This situation can end in one of two ways:

**Without a roadmap**: "Oh yeah we’ll get right on that!"
  - Developers drop everything they're doing to work on the new ask. Current features are put on hold. External stakeholders that were counting on the current features to be completed are upset because other features were prioritized over theirs.
  - This turns in to a situation of "the squeaky wheel gets the grease," where teams that cry the loudest get their features worked on first.
  - Engineering finds it difficult to make long-lasting design decisions, since new asks come in at a frantic pace and everything always seems like it is on fire.

**With a roadmap**: "Check the roadmap"
  - The new team checks the roadmap to see when the feature was previously planned to be completed. Usually they will just check back later at this point, since their ask wasn't as urgent as they made it out to be.
  - If the ask truly is urgent, the new team can work with product to see if any short-term priorities can get shifted to meet their needs.
  - This allows developers to focus on the roadmap, work at a reasonable pace, and continue to make design decisions that will facilitate implementation for the foreseen medium-term goals.

The roadmap winds up working like **a shield for the development team**, deflecting unplanned feature asks until they can be properly worked in to the roadmap itself.

[roadmap]: https://www.aha.io/roadmapping/guide/product-roadmap
[dp]: https://developer.uptake.com
[aha]: https://www.aha.io/

