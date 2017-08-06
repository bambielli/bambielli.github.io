---
title:  "Cohort Analysis"
date:   2016-03-18 13:43:00
category: til
tags: [measurement, product-strategy, marketing]
---

TIL about cohort analysis, and how it can be used as an input to product strategy.

## What is Cohort Analysis?

Cohort Analysis is a way to examine data by grouping it by the profile of the customer that generated it. By looking at data in this way, you can gain insight in to any trends or patterns that are specific to a certain type of customer.

## How Are We Using It?

We are beginning to implement usage / event tracking throughout our application using [Keen.io][keen]{:target="_blank"}. Keen allows us to submit complex objects with each event we submit to the service, including user profile information like their access level and user_id, so we can group events during our analysis by the type of user who generated them. We hope to use this usage data to better understand which types of users are prone to use our application, better understand the value that we offer to these users, and try to improve on the value we offer to different user types.

## Strategy

My hypothesis is that shortly after launching a dashboard, the usage statistics for all types of user profiles drops off to 0 and stays there, as there is currently no incentive to come back to the application after seeing our initial analysis. We have a few thoughts on how to make the app more of an everyday essential type of tool, and using cohort analysis ***we should be able to tell if what we are doing resonates with any of our core customer profiles by seeing usage data level off asymptotically with the X axis instead of dropping through it.***

Once we reach this point (we have something that at least one type of user uses daily) we can focus on one of the following:

  1. Getting the usage graphs for other user types to level off asymptotically - different types of users find daily utility with our tool.
  2. Raising the asymptote of one particular user type higher on the Y axis - *more* of a single type of user finds utility with our tool.

The strategy we take here is up to the product owner, but collecting and analysing this data is a crucial step towards understanding if what we are building is actually providing incremental value to our customer base.

[keen]: https://keen.io

