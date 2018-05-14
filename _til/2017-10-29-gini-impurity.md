---
title:  "Gini Impurity (With Examples)"
date:   2017-10-29 20:44:00
category: til
tags: ["machine", "learning", "machine learning", "gini", "information", "gain", "entropy", "impurity"]
---

TIL about Gini Impurity: another metric that is used when training decision trees.

Last week I learned about [Entropy and Information Gain][ig]{:target="_blank"} which is also used when training decision trees. Feel free to check out that post first before continuing.

I will be referencing the following data set throughout this post

```
"Will I Go Running" Data Set

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

## Gini Impurity

`Gini Impurity` is a measurement of the likelihood of an **incorrect classification** of a new instance of a random variable, if that new instance were **randomly classified according to the distribution of class labels** from the data set.

Gini impurity is **lower bounded by 0**, with 0 occurring if the data set contains only one class.

The formula for calculating the gini impurity of a data set or feature is as follows:

```
        J
G(k) =  Σ P(i) * (1 - P(i))
       i=1
```

Where P(i) is the probability of a certain classification `i`, per the training data set.

If it seems complicated, it really isn't! I'll explain with an example.

## Example: "Will I Go Running?"

In the data set above, there are two classes in which data can be classified: "yes" (I will go running) and "no" (I will not go running).

If we were using the entire data set above as training data for a new decision tree (not enough data to train an accurate tree... but let's roll with it) the `gini impurity` for the set would be calculated as follows:

```

G(will I go running) = P("yes") * 1 - P("yes") + P("no") * 1 - P("no")

G(will I go running) = 4 / 7 * (1 - 4/7) + 3 / 7 * 1 - P(3/7)

G(will I go running) = 0.489796

```

This means there is a **48.97%** chance of a new data point being incorrectly classified, based on the observed training data we have at our disposal. This number makes sense, since there are more `yes` class instances than `no`, so the probability of mis-classifying something is less than a coin flip (if we had the same number).

So how do we use this when building a decision tree?

## Gini Gain

Similar to entropy, which had the concept of `information gain`, `gini gain` is calculated when building a decision tree to help determine which attribute gives us the most information about which class a new data point belongs to.

This is done in a similar way to how information gain was calculated for entropy, except instead of taking a weighted sum of the entropies of each branch of a decision, we **take a weighted sum of the gini impurity.**

```
Gini_Gain(attribute) = total_impurity - impurity_remainder(attribute)

                     branch_n
remainder(attribute) = Σ P(attribute_branch_n)*G(branch)
                     branch
```

## So which should I use? Gini Impurity or Entropy?

It seems that `gini impurity` and `entropy` are often interchanged in the construction of decision trees. **Neither metric results in a more accurate tree than the other.**

All things considered, a slight preference might go to `gini` since it doesn't involve a more computationally intensive `log` to calculate.

[ig]: /til/2017-10-22-information-gain/
