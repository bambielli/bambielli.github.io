---
title:  "Applying Supervised Learning to Addiction Medicine Data"
date:   2018-02-11 16:07:00
category: post
tags: [addiction, medicine, supervised, learning, KNN, K-NN, decision, tree, trees, neural, network, networks, nearest, neighbor, neighbors, support, vector, machine, machines, adaboost, boosting, ensemble, above and beyond, AnB]
---

I spent the last few weekends applying 5 different supervised learning models to an anonymized and labeled set of data representing individuals of an addiction & family medicine clinic in the Chicago area.

See my full analysis here: [Applying Supervised Learning to Addiction Medicine Data][paper]{:target="_blank"}

## Background

I am working with a former professor of mine from Northwestern, who volunteers at a local Chicago clinic as a tech consultant. He provided me with some (anonymized) data that represents demographic, background, and diagnosis information for Clients of the clinic. **He's hoping I can help uncover factors in the data that indicate whether a new client of the clinic is likely to complete their prescribed programming**, as currently the graduation rate for the program hovers around 20%.

This partnership was well timed, as I was looking for an interesting data set to work with for the first assignment of my [Georgia Tech Machine Learning][ml]{:target="_blank"} class anyways!

## Methodology

Assignment 1 for the class involved training `Decision Trees`, `Neural Networks`, `K-Nearest-Neighbor` (KNN) instance-based classifiers, `Boosted Decision Tree` ensemble learners, and `Support Vector Machines` on two structurally different data sets. After training the 5 different classifiers, we were to compare the performance of the classifier against each data set and additionally against each other model.

The first data view I used was left raw and relatively un-processed from the data provided by my professor, with a total of around 8000 sparse attributes after `One Hot Encoding` of categorical features. I derived a boolean column that represented whether the instance completed their programming, which I used as the classification label, but otherwise Data View 1 was raw.

I applied a cleaning and dimension reduction procedure to create the second data view, that **reduced the number of dimensions in the clinic data from 8000 down to 118**.

The difference in dimensionality between these two data sets was purposeful, as certain supervised learning methods perform better as the dimension space shrinks (e.g. KNN and the `curse of dimensionality`), while others can perform better with more information (e.g. neural networks). I was hoping to highlight these differences in my analysis.

I calculated learning curves for each trained model to get an estimate of how model accuracy changed with the number of training instances. I selected the best performing models from different parameter combinations using `k=5 folds cross-validation`, to ensure that the best performer wasn't just performing well due to a chance selection of training and test sets.

Model performance was gauged based on accuracy in classifying the training set, which is a pure measure of the number of combined correct "True" and "False" classifications in the test data set.

## Results

While the analysis was educational for myself, I don't think this first round will be very useful for my professor and his ultimate question of "what factors influence the completion of prescribed clinic programming?"

Except for decision trees, the other models I trained are black-box in their results: they provide a trained classifier with some degree of accuracy, but don't give the model developer any insight in to how classification decisions are made.

Even if you were to peek in to the hidden layers of a neural network, or get the equation of the function used to divide classes with Support Vector Machines, **the values don't provide any insight in to which attributes are ultimately the most impactful when making a classification decision.**

Regardless of the accuracy (and the accuracy was over 97% in some cases!) giving my professor a classifier without insight in to why the classifier makes its decisions does not help his cause.

Furthermore, I purposefully chose two very different views of the clinic data: one raw, and one with highly derived data. This was for the sake of comparing model performance on two structurally different data sets. **There is absolutely a happy medium between these two views** that removes noise without losing out on the nuance in the data, which would likely improve model performance over what I observed.

I have ideas on how to tune my cleaning procedure to do this, and now realize that **it is often better to be conservative when pruning data than too aggressive.**

I'm optimistic that I'll be able to find something interesting to help my professor's cause, but this first round was mostly for my own education and for getting acquainted with the data itself. **There are 3 more assignments for class this quarter**, which will require me to look at the same data with different methods (some more supervised, but also unsupervised).

I also plan on running some more "white-box" supervised learning algorithms on the data, like `linear regression` and even decision trees again. I'm hoping this gives me a better sense of the most influential factors in making the "graduate" vs "not graduate" decision.

I also think something like [F1 score][f1]{:target="_blank"} would be a better choice to assess model performance instead of pure accuracy, since "False" instances dominate my data set (over 80% False).

If you're interested in **seeing the code**, or have any questions about my experience in the Georgia Tech masters program thus far, feel free to reach out!

<embed src="/assets/pdf/assignment-1-ml.pdf" />

[paper]: /assets/pdf/assignment-1-ml.pdf
[ml]: https://www.omscs.gatech.edu/cs-7641-machine-learning
[f1]: https://en.wikipedia.org/wiki/F1_score
