---
title:  "Information Entropy and Information Gain"
date:   2017-10-22 18:00:00
category: til
tags: ["information", "gain", "entropy", "theory", "bits", "architecture", "machine learning", "AI"]
---

TIL about Information Entropy and Information Gain, which are key pieces towards determining the relevance of a decision when constructing a decision tree.

## Wait... Information has Entropy?

Before now I always thought about entropy in the context of the physics domain, as in the degree of randomness in a physical system.

After studying decision trees as part of my AI and Machine Learning course through Georgia Tech, **I learned that entropy could be used to describe the amount of unpredictability in a random variable.**

The `Entropy` of a random variable can be calculated using the following formula:

```
        d
B(k) = -Σ P(k_d) * log2(P(k_d))
        1
```

Where `k` is an attribute of your decision tree.

A few examples of entropy values:

  - A **fair coin** has an entropy value of **1 bit**, because the H/T values are equally weighted and can be encoded by a single bit of information.

  - A **weighted coin** with 99% probability of heads an 1% of tails has an entropy value of **0.08 bits**. There is clearly less randomness in this weighted coin, so it makes sense that the required information to store the result of this coin is much lower.

Say I have data describing the random variable `Will I go Running?`. Each row of the table represents a day of the week `Day1 - Day7` where I may or may not have gone running.

Each row has 3 attributes: `Weather`, `Just Ate`, and `Late at Work` which influence the Yes/No decision of whether I go running.

```
Day   | Weather | Just Ate | Late at Work | Will I go Running?
---   | ---     | ---      | ---          | ---
1     | 'Sunny' | 'yes'    | 'no'         | 'yes'
2     | 'Rainy' | 'yes'    | 'yes'        | 'no'
3     | 'Sunny' | 'no'     | 'yes'        | 'yes'
4     | 'Rainy' | 'no'     | 'no'         | 'no'
5     | 'Rainy' | 'no'     | 'no'         | 'yes'
6     | 'Sunny' | 'yes'    | 'no'         | 'yes'
7     | 'Rainy' | 'no'     | 'yes'        | 'no'
```

Calculating the `total entropy` of the system, or the **total amount of variability in the random variable**, is possible by summing the entropy of a goal (a `yes` response that I will go running) and the entropy of a negative result (a `no` response that I will go running).

The probabilities can be read out of the data table itself, by counting.

```
B(Will I go Running) = -p('yes')log2(p('yes')) - p('no')log2(p('no'))

B(Will I go Running) = -(4/7)log2(4/7) - p(3/7)log2(3/7)

B(Will I go Running) = 0.985228136034

```

So the total entropy for the variable Will I go running is a small amount less than 1, indicating that there is **slightly less than a 50% chance that the decision to go running will be yes/no**.

## Information Gain

How is this useful when constructing decision trees?

**Entropy is used when determining how much information is encoded in a particular decision.** This `information gain` is useful when, upon being presented with a set of attributes about your random variable, you need to decide on which attribute tells you the most info about the variable.

When building decision trees, **placing attributes with the highest information gain at the top of the tree** will lead to the highest quality decisions being made first. This will result in **more succinct and compact decision trees**.

## Calculating information gain

A quick plug for an [information gain calculator][ig]{:target="_target"} that I wrote recently. Check it out, and continue reading to understand how it works.

We've already calculated the total entropy for the system above. The next step is to **calculate the entropy remainders from that total entropy after each attribute in the data set is processed and data is classified.**

`Information Gain` is calculated as following:

```
Gain(attribute) = total_entropy - remainder(attribute)

                     branch_n
remainder(attribute) = Σ P(attribute_branch_n)*B(branch)
                     branch
```

When calculating the remainder for an attribute, **you iterate over the count of goal values classified by each branch of your attribute**. In the data table above, when looking at the Weather attribute, each branch would contain counts of "yes" and "no" `Will I Go Running` values classified by the 'sunny' and 'rainy' branches of the Weather attribute.

`P(attribute_branch_n)` is the number of goal values classified by the branch of the attribute, divided by the total number of entries in the data table. `B(branch)` is the entropy of the branch of the attribute itself.

In the example above, the remainder and information gain for the `Weather` attribute can be calculated as follows:

```
remainder(Weather) = P('Sunny and yes running')*B('Sunny') + P('Sunny and no running')*B('Sunny') + P('Rainy and yes running')*B('Rainy') + P('Rainy and no running)*B('Rainy')

remainder(Weather) = (2/7)*B('Sunny') + (1/7)*B('Sunny') + (2/7)*B('Rainy') + (2/7)*B('Rainy')

remainder(Weather) = 0.463587499691
Gain(Weather) = 0.985228136034 - 0.463587499691 = 0.5216406363433186
```

The information gain for the Weather attribute is 0.5216 bits, which is **over half of the information stored in the random variable**.  For comparison, here are the gains for all 3 attributes together:

```
Gain('Just Ate') =  0.02024420715375619
Gain('Weather') = 0.5216406363433186
Gain('Late at Work') = 0.12808527889139454
```

The Weather attribute tells us the most about our `Will I Go Running` random variable, since its information gain is the highest of the 3 available attributes.

If we were to construct a decision tree for classifying new instances of this random variable, **we would want to place the Weather attribute at the top of the tree** since it contains the most information about the inevitable result of the variable.

[ig]: https://github.com/bambielli/InformationGain
