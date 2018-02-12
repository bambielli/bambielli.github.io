---
title:  "When to Use One Hot Encoding"
date:   2018-02-11 20:49:00
category: til
tags: [one hot encoding, one-hot-encoding, one, hot, encoding, preprocessing, pre-processing, machine learning, data science, label encoding, OneHotEncoder, LabelEncoder]
---

TIL about One Hot Encoding, and when it is necessary to use as a preprocessing step for machine learning models.

## What is One Hot Encoding?

`One Hot Encoding` is a pre-processing step that is applied to categorical data, to convert it into a non-ordinal numerical representation for use in machine learning algorithms.

Phew there's a lot to unpack there! Let's go through an example.

## Step 1: Convert Categorical Data to Numerical Labels

Let's say we have 3 data instances with attributes of `Preferred Programming Language` and `OS of Choice`.

Preferred Programming Language | OS of Choice
------------- | ------------- |
Javascript | OSX |
Python | Linux |
Scala | OSX |

Machine learning algorithms provided by libraries like sklearn **have trouble working with attributes with string values** (like the attributes above). To fix this problem, we perform a pre-processing step that converts string valued attributes in to numerical representations.

sklern has a [`LabelEncoder`][le]{:target="_blank"} that does just that: turns string values in an attribute in to numerical values.

After running the data above through the sklearn `LabelEncoder`, our table will look something like this:

Preferred Programming Language | OS of Choice
------------- | ------------- |
0 | 0 |
1 | 1 |
2 | 0 |

## The Problem Of Ordinality

Stopping here, though, **causes a problem...** machine learning algorithms will treat the `ordinality` of numbers in an attribute with some significance: **a higher number "must be better" than a lower number** in some way.

For some categories, this can make sense: if "cold" is better than "warm" is better than "hot", for example, maybe stopping at the LabelEncoded representation makes sense. In the temperature case, **it can actually convey more information than the un-encoded version.**

For most categories, though, there is no sense of superiority between category values, and the `ordinality` injected by the `LabelEncoder` just results in noise.

Fortunately, there is a way to combat this: `One Hot Encoding`

## One Hot Encoding

`One Hot Encoding` takes an attribute with numerical values, and encodes the values as binary arrays. The length of these arrays is the max value of the numerical category.

The result of a One Hot Encoded attribute is **n binary attributes** that represent the values in the original attribute. This allows a machine learning algorithm to leverage the information contained in a category value **without the confusion caused by ordinality**.

sklearn offers a [`OneHotEncoder`][ohe]{:target="_blank"} class in its preprocessing package.

A One Hot Encoded version of the Label Encoded table above would look something like this:

Javascript | Python | Scala | OSX | Linux |
---------- | ------ | ----- | --- | ----- |
1          | 0      | 0     |  1  |   0   |
0          | 1      | 0     |  0  |   1   |
0          | 0      | 1     |  1  |   0   |

Notice that we now have 5 binary attributes, one for each of the values from the original 2 attributes. 3 were created from the original attribute "Preferred Programming Language" and 2 from the original attribute "OS of Choice". **A 1 represents that the instance had that category value.**

Also recognize that while one hot encoding reduces noise in your data that would have otherwise been cause by incorrect ordinal relationships, **it also greatly increases the dimensionality of your data.** High dimensionality has its own set of problems in machine learning, such as the `curse of dimensionality` (which I plan on writing about in the next few days).

In a project I was working on recently, some category attributes had hundreds of choices. While there were only 20 attributes to start, there were **close to 8000 attributes** after one hot encoding. That's a huge, but necessary increase in dimensionality to support categorical data in your models.

The new binary attributes created from One Hot Encoding will also be quite `sparse` (mostly zeros) unless your attribute allows an instance to take on multiple values.

[le]: http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html
[ohe]: http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html
