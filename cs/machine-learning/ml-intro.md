---
description: >-
  Introduction to basic concepts of machine learning. // TODO: add link for
  Markov Process
---

# \*Introduction

## Overview

Machine Learning is a subject to study in **how to make computer behave human-like learning to make progress and become greater.** This's the core of Artificial Intelligence.

> A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its  performance at tasks in T, as measured by P, improves with experience E.    
>
>                                                                           -- 1959 by Arthur Samuel. Tom M. Mitchell

## Research Significance

Machine learning has a wide range of applications: **data mining, computer vision, NLP, biometrics, search engines, medical diagnostics, detection of credit card fraud, securities market analysis, DNA sequence sequencing, speech and handwriting recognition, strategy games and robotics.**

## Machine Learning Scenarios

| Pattern Recognition | Machine Learning | Deep Learning |
| :--- | :--- | :--- |
| Basic Standard | Data Learning | In-depth Data Learning |
| Based on massive data | Based on surfacial features | Based on in-depth features |
| "Past" | "Now" | "Future" |

## Machine Learning Composition

### Supervised Learning

| Classification | Regression |
| :---: | :---: |
| Classifies data into appropriate categories | Predict result |

* Already known expected output.
* Dataset = Training set + Test set
  * Training set = feature + label \(discrete for classification, continous for regression\)
  * Feature is normally from training data's collumn.
  * label is the result. 

### Unsupervised Learning

Don't know expected output.

| Clustering | Density Estimation | Dimensionality Reduction |
| :---: | :---: | :--- |
| Divide data to multiple classes. | Estimate the similarity to the group according to the closeness of the sample distribution. | Reduce the dimensionality of data features. |

### Reinforcement Learning

It's concerned with the problem of finding suitable actions to take in a given situation in order to maximize a reward by a process of trial and error.

{% hint style="info" %}
A general feature of reinforcement learning is the trade-off between **exploration** and  **exploitation**.

* **Exploration:** Try out new kinds of actions.
* **Exploitation:** Make use of actions that are known to yield a high reward.
{% endhint %}

