---
description: >-
  What are algorithms? Why is the study of algorithms worthwhile? What is the
  role of algorithms relative to other technologies used in computers?
---

# 1 The Role of Algorithms in Computing

## 1.1 Algorithms

_**What are algorithms?**_

Informally, an **algorithm** is any well-defined computational procedure that takes some value, or set of values, as **input** and produces some value, or set of values, as **output**.

An algorithm is thus **a sequence of computational steps** that **transform the input into the output**.

{% hint style="info" %}
**How to formally define the** _**sorting problem:**_

**Input:** A sequence of $$n$$numbers $$<a_1,a_2,...,a_n>$$.

**Output:** A permutation \(reordering\) $$<a_{1}^{'},a_{2}^{'},...,a_{3}^{'}>$$ of the input sequence such that $$<a_{1}^{'}\leq a_{2}^{'}\leq \dots \leq a_{3}^{'}>$$.
{% endhint %}

An algorithm is said to be _**correct**_ if, for **every** input instance, it halts with the correct output. We say that a correct algorithm _**solves**_ the given computational problem.

{% hint style="warning" %}
**Note:** Incorrect algorithms can **sometimes** be useful, if we can control their error rate.
{% endhint %}

_**What kinds of problems are solved by algorithms?**_

* The Human Genome Project
* The Internet: finding good routes on which the data will travel.
* Electronic commerce.

### Hard problems

_**NP-complete problems:**_

1. No one knows whether or not efficient algorithms exist for NP-complete problems.
2. If an efficient algorithm exists for any one off them, then efficient algorithms exist for all of them.
3. Several NP-complete problems are similar, but not identical, to problems for which we do known of efficient algorithms.

## 1.2 Algorithms as a technology

Computing time is therefore a bounded resource, and so it space in memory. Algorithms that are efficient in terms of time or space will help to use resources wisely.

**Efficiency**

{% hint style="info" %}
Computer A: The resulting code requires $$2n^2$$ instructions to sort $$n$$ numbers.

Computer B: The resulting code taking $$50n lg n$$ instrucitons.

To sort 10 million numbers, 

computer A takes $$\frac{2 (10^7)^2 instructions}{10^{10} instructions / second} = 20,000 \ seconds$$

computer B takes $$\frac{50(10^7)lg^{10^7} instructions}{10^7 instructions / second} \approx 1,163\ secondds$$
{% endhint %}

By using an algorithm whose running time grows more slowly, even with a poor compiler, computer B runs more than 17 times faster than computer A.

**Algorithms and other technologies**

Total system performance depends on choosing efficient algorithms as much as on choosing fast hardware.

