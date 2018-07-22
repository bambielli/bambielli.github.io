---
title:  "The Perils of Underestimating Stories"
date:   2018-07-22 11:36:00
category: til
tags: [stories, underestimation, overestimation, agile, project management]
---

TIL that while the exact number a team uses to estimate a story is meaningless and varies from team to team, the accuracy of the relative estimate matters quite a lot: underestimation winds up being harmful, while overestimation is relatively benign.

## The Scenario

Our team recently estimated a set of stories for a data table component. One of the features of that table, a `fixed-header` that stays in view as the user scrolls down the page, was estimated as a 3 (on the low end of our scale).

When implementation began, however, it actually wound up being more like an 8's worth of work. We use a Fibonacci scale to estimate tasks, so a 3 to an 8 indicates a miss of 2 factors of difficulty on our scale.

I can already hear the chorus of `"who cares???"` being shouted at the screen: story points are meant to be inaccurate and rough estimates! There are likely scenarios where you overestimate stories as well (maybe some are estimated as an 8 that are really a 3) so **doesn't it all just balance out in the end?**

From a point perspective, it absolutely does balance out, but there are additional impacts of severe underestimation that can derail a sprint and cause a development team to miss on one of its key performance indicators: `team predictability`.

## Predictability is Key

Every sprint, a development team signs up to complete a certain number of stories. Points are put in place for the business to know how many stories are reasonable for a development team to commit to. It is also a safety mechanism for the development team itself, to ensure they don't commit to too much work during a sprint.

**If a team completes what they signed up for, that means they are predictable.**

- The business can count on them to achieve what they say they will, on-time.
- This allows product managers to produce better forward-looking estimations during [roadmap creation][roadmap].

**If a team consistently misses on what they commit to complete, the team is not predictable.**

- The business cannot count on the team to produce features on time, which leads to an inability for the product team to plan.

**Predictability** should be a key measurement in the success or failure of a development team.

## How does Overestimation Impact Predictability?

Let's run through a scenario: **a story was estimated as an 8 when in actuality it was a 3.**

What is the impact?

- During sprint planning the team will commit to fewer stories during the sprint.

- When the overestimated story is pulled by a team member, they will finish it well ahead of time.

- This will open up more capacity in the sprint to potentially pull in more work than was originally committed to (perhaps another 5-point story).

- The team predictably delivers the stories they committed to, and wow! They even pulled in and completed more work than they originally said they would!

Everyone rejoices over beers after sprint demo at such a productive and predictable team.

## What about Underestimation?

Now a new scenario: **a story was estimated as a 3 when in actuality it was an 8.**

What is the impact?

- During sprint planning the team will pull in this underestimated story along with another 5-point task.

- They might not start the underestimated 3-point task until later in the sprint as they prioritize other higher-pointed items first.

- When implementation begins, the team realizes the story is more of an 8.

- The story doesn't finish during the sprint, and other committed stories might be tangentially impacted.

No rejoicing or beers at the end of the sprint... just more work and unpredictability.

## The Perils of Underestimation

The differences between over and underestimation are clear: one leads to a predictable team and the other to an unpredictable one.

I think an easy trap for development teams to fall in to, is the urge to give low estimates on the difficulty of stories to somehow prove to management that they are "strong technically".

Management doesn't care, they just want to know what they can expect the team to deliver next sprint so they can report up: predictability trumps all. I'd rather work with a predictable team that delivers 10 points per sprint, than a team that delivers 20 stories one sprint, then signs up for another 20 and only completes 5.

The value of predictability can't be understated.

## How to Combat Underestimation?

Underestimation generally stems from a fundamental lack of understanding by the development team about the necessary steps to build a feature. The team thought something would be easy, but wound up being hard for any number of unknown reasons.

In our case, when we estimated our data table feature as a 3 and it was more of an 8, there was only one team member who had touched the data table component before. He was out of the office the day the feature was estimated.

Our team's rule going forward is:

- **if a story is underestimated by more than 2 factors (e.g. 3 -> 8), we have a short team discussion after standup about what wound up being unexpectedly challenging about the implementation**.

This might involve code walkthroughs with the team, to raise overall familiarity in an unfamiliar part of the codebase.

Another way we combat underestimation is a team principle to be **purposefully conservative with our estimates**.

If we've never worked in a part of the codebase before, we purposefully estimate feature requests for that component a size higher, due to unknowns. Like we saw in the scenarios above, overestimation doesn't have the same perils as underestimation, and still leads to predictable development team.

## Is Resizing Stories Post-Implementation Worth It?

I would argue yes, since it helps illustrate a more accurate measure of our team velocity over time. Others argue no, since it all "balances out" in the end anyways, but I think re-estimating helps keep the team honest with their historical degree of commitment each sprint.

[roadmap]: /til/2018-02-18-why-roadmaps-are-important
