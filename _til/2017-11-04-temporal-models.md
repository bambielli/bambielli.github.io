---
title:  "Temporal Models"
date:   2017-10-29 20:44:00
category: til
tags: ["machine", "learning", "machine learning", "artificial", "intelligence", "artificial intelligence", "ai", "AI", "A.I", "math", "probability", "statistics"]
---

TIL how to create a temporal model of a system using some simplifying assumptions for ease of computation.

## What is a Temporal Model

A `temporal model` is a representation of a stochastic process **over time.**

In a temporally modeled system, the world is described as a series of snapshots, or `time slices`. Each slice is a distinct view of the world at time `t`. We can use time slices collected during observation of the system to perform `inference`, and make predictions on future states or re-calculate past states with better accuracy.

Temporal models have sets of possible `states`, `transitions` between those states (and their likelihood), and sets of `evidence` (observations) that provide clues as to what state the system is currently in.

For example, consider a system where the state `X` consists of the random variable `R`, indicating whether it is raining at time `t`. Observable evidence for this system might include whether a coworker brought an umbrella to work `U`, which influences knowledge about `R` at time t when the umbrella observation was made.

## Transitions Models and Markov Processes

The `transition model` (how the world evolves over time) for a system is specified as `P(Xt | X0:t-1)`, or, in other words, **the probability of X at time t=now given the probability of X at all times prior to now**. This poses a problem: since t can be infinitely large, X0:t-1 can become extremely difficult or impossible to calculate.

This issue is solved by making a simplifying assumption known as a `Markov Assumption`: **the current state at time t only depends on a finite fixed number of previous states from time 0 <= k < t.**

The most simplifying of these assumptions is that the current state only depends on the direct predecessor `Xt-1` (one prior state). A system modeled in this way is called a `first-order markov process`, and the probability of Xt can be simplified to `P(Xt | Xt-1)`. When you make this assumption, you are suggesting that knowledge of the previous state makes the future conditionally independent of the past. To write this statement as a probability formula: `P(Xt | X0:t-1) = P(Xt | Xt-1)`.

To bring it back to our weather predicting example above of Rain at a given time: if we model this system as a first-order markov process, we are implying that rain two days (assuming units on t is day) that it does not have any impact on whether it will rain today.

Note that in some situations, modeling a temporal system as a first-order markov process is too simplifying... to increase the accuracy of your model, you can **evolve your model** into a second or third order markov process (where systems depend on more than 1 prior state). You can also **increase the set of state variables** you are tracking by adding additional sensors, which will provide more information about the world at time `t` allowing you to make better approximations of the

## Sensor Model

After specifying a transition model for our system, we also need to specify a `sensor model` for how our sensors detect information about the current state in the form of Evidence variables.

Sensors *could* be influenced by previous states in the system, or even previous evidence variables collected by other sensors, but we make another `Markov Assumption` for the sake of simplification: **evidence variables are produced only by the state at current time t, and are not influenced by other collected evidence variables.** To write this statement as a probability formula: `P(Et | X0:t, E0:t-1) = P(Et | Xt)`.

Let's apply the sensor model above to our weather prediction system. In the case of our Evidence variable `Umbrella`, the simplifying markov assumption applied to our sensor model means that if it rained yesterday our coworker would not have left their umbrella at work so it would still be present today. If we observe an umbrella today at time `t`, our markov assumption for our sensor model implies that it could only have been caused by a rain event R today at time `t`.

## Joint Probability at time t

Once we've defined our `transition model` (how our system evolves) and our `sensor model` (how our evidence variables get their values) we can calculate the joint probability distribution of our states X and evidence variables E at time t with the following formula:

```
                      t
P(X0:t, E1:t) = P(X0) Π P(Xi|Xi-1)*P(Ei|Xi)
                     i=1
```

Where `P(Xi|Xi-1)` represents our first-order markov process transition model and `P(Ei|Xi)` represents our simplified sensor model. `P(X0)` is the probability of X at time t=0, which kicks off the state of the world before observations begin.
