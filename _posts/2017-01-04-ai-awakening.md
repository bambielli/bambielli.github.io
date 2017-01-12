---
title:  "AI Awakening NYT"
date:   2017-01-11 19:41:00
category: post
tags: [ai, machine-learning]
---

Everyone can see the impact A.I. is having our lives, but an article I read in NYT magazine titled ["The Great A.I. Awakening"][ai]{:target="_blank"} hammers home just how impressive the changes could be within our lifetime.

Here are a few of my takeaways from the article.

### A Differnt Way to "Program"

A Neural Network is a series of computational steps that accepts one-to-many factors as inputs, processes these inputs, and produces a single output that may be used in subsequent computations. These systems are modeled after neurons in the human brain (hence "Neural" Networks) and are designed to solve problems that humans excel at but that are difficult to express via traditional programmatic steps. An example of a problem that humans excel at is feature detection or pattern recognition (i.e. does this picture contain a dog).

Neural networks are trained from the ground up, "learning" from vast amounts of input data and drawing conclusions from it. This contrasts with traditional programming, which institutes a "top down" set of rules for a computer to follow, which the computer will follow without deviation. Learning from training data allows a neural network to make probabilistic decisions on the likelihood that input meets the criteria to which the network has been trained.

An example, if the goal of a program is to identify if a picture contains a dog: the "bottom up" approach used to train a neural network for this task would involve feeding the network millions of stills of different kinds of dogs as training data. Having processed these stills, the network will build an "understanding" (so to speak) of what a dog looks like, and will be able to make generalizations about new inputs.

The "top down" approach would involve programming an exhaustive ruleset for what defines a dog that covers every possible edge case imaginable. You can tell right away that this method is not scalable or maintainable... every time a dog picture deviates slightly from the programmed ruleset (i.e. a dog with 3 legs instead of 4) the program will need to be modified to correctly perform this edge case. A data trained neural network will always outperform a traditionally programmed computer for problems like these, so long as the data set is sufficiently large to allow the neural network to be confident in its predictions.

Different training methods involve varying levels of tuning of components in the network: supervised learning, for example, permits the network architect to manually weight components in the net to achieve optimal results.

### Exponential Improvements

For problems that would benefit from machine learning applications, the improvements can be dramatic. The example discussed in the NYT article was improvements to Google Translate that happened early 2016.

A team from Google Brain proposed that they swap out part of the translate application logic with a neural network trained for translation purposes. The results were remarkable: the improvement to the quality of translations after implementing the neural network was "roughly equivalent to the aggregate gains of the old system over its entire lifetime of development." A decade of development using traditional programming techniques was made obsolete in 9 months of neural network training: an unbelievable achievement. These types of dramatic improvements are not unique, and will lead to revolutions in the types of problems we can efficiently solve.

### Garbage in, Garbage out

One concern that the NYT article raised was training a neural network on biased data... the developer’s adage when writing an algorithm is "garbage in, garbage out," meaning if you provide an algorithm with bad data it will surely give you wacky and incorrect results back.

A neural network trained on data this is inherently biased will fall prey to the same biases contained in the data set when making its decisions. This could have huge impact for an application, leading to skews in how results are processed or calculated (or detected) based on the input data it was trained on.

I did not realize that the quality of data used for training was so critical to the overall function of the network.

### Summary

This article piqued my interest in machine learning / A.I. applications. I'm currently in the process of applying to the Georgia Tech Online Masters in Computer Science program, and I'm hoping to pursue the Machine Learning specialization. The impact machine learning will have in the future will be extraordinary: I want to make sure I'm equipped as a developer to participate in solving these types of problems!

[ai]: http://www.nytimes.com/2016/12/14/magazine/the-great-ai-awakening.html

