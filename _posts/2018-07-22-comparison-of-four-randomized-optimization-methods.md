---
title:  "Comparison of Four Randomized Optimization Methods"
date:   2018-07-22 11:13:00
category: post
tags: [randomized, optimization, randomized optimization, random hill climbing, hill climbing, simulated annealing, simulated, annealing, genetic algorithms, genetic, algorithm, mimic, count ones, four peaks, knapsack, np-hard]
---

This post compares the performance of 4 different randomized optimization (RO) methods in the context of problems designed to highlight their strengths and weaknesses.

## Randomized Optimization Methods

The four RO methods explored were:

- [`Random Hill Climbing`][rhc]{:target="_blank"} - a standard hill climbing approach where optima are found by exploring a solution space and moving in the direction of increased fitness on each iteration.

- [`Simulated Annealing`][sa]{:target="_blank"} - a variant on random hill climbing that focuses more on the exploration of a solution space, by randomly choosing sub-optimal next-steps with some probability. This increases the likelihood of finding global optima instead of getting stuck in local optima.

- [`Genetic Algorithms`][ga]{:target="_blank"} - a subset of evolutionary algorithms that produce new generations based on fitness of prior generations.

- [`MIMIC`][mimic]{:target="_blank"} - An RO approach created by professor Isbel of Georgia Tech, that attempts to exploit the underlying "structure" of a problem to eliminate re-exploration of sub-optimal portions of the solution space on future iterations.

## Problem Contexts

The 3 problems I chose, which highlight the strengths and weaknesses of these algorithms, were:

- `count ones` - a simple problem with a single global optima with large basin of attraction. SA and RHC should excel here, since their evaluation functions are inexpensive to compute and there are no local optima in which to get stuck.

- [`four peaks`][4p]{:target="_blank"} - A problem with two local optima with wide basins of attraction designed to catch simulated annealing and random hill climbing, and two sharp global optima at the edges of the problem space. Genetic Algorithms are more likely to find these global optima than other methods.

- [`knapsack`][knapsack]{:target="_blank"} - a classic NP-Hard optimization problem with no polynomial time solution. The strength of MIMIC was highlighted in this context, as it exploited the underlying structure of the problem space that was learned from previous iterations.

The implementations of these algorithms and problem scenarios were pulled from the [ABIGAIL][abigail]{:target="_blank"} library, which is maintained by Pushkar Kohle of Georgia Tech.

## Analysis

[Click here][paper]{:target="_blank"} for the full paper with more detail and analysis, or view it below.

<embed src="/assets/pdf/Comparison-Of-Four-Randomized-Optimization-Methods.pdf" />

[rhc]: https://en.wikipedia.org/wiki/Hill_climbing
[sa]: https://en.wikipedia.org/wiki/Simulated_annealing
[ga]: https://en.wikipedia.org/wiki/Genetic_algorithm
[mimic]: https://www.cc.gatech.edu/~isbell/tutorials/mimic-tutorial.pdf
[knapsack]: https://en.wikipedia.org/wiki/Knapsack_problem
[4p]: https://pdfs.semanticscholar.org/cd4f/e89d8dd6060e2957041f90fc699a30058d01.pdf
[abigail]: https://abagail.readthedocs.io/en/latest/index.html
[paper]: /assets/pdf/Comparison-Of-Four-Randomized-Optimization-Methods.pdf
