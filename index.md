---
title: "Evaluation Tool Overview"
keywords: Bayesian Network Evaluation Validation
tags: [getting_started]
sidebar: mydoc_sidebar
permalink: index.html
summary: This brief introduction will introduce you acessing the tool, as well as arguing it's necessity
---

## Acessing the tool

The tool can currently be accessed [here](http://surgo-project.herokuapp.com/eval)

## Why do we need an evaluation tool?

Bayesian networks are an extremely powerful modelling technique which have the ability to create **simulated worlds**, where the user has the ability to assert complete control over a set of variables. The effect of this is that a user can **intervene** on the **learnt** model of the sub-world (the variable space of the collected observations), and perform experiments to determine the causal effect of these interventions. Effectively, this is the equivalent to performing a costly and potentially unethical RCT, given the model is **interventionally sufficient**.

Unlike traditional machine learning, with Bayesian networks we lack to ability to sufficiently **validate** any learnt models. Typically, a held-out test set from the data would be compared against any predictions of a learnt model – comparing predicted against true. When learning a Bayesian network model however, we are required to identify the **structure** of the variable interactions, which we have no true case to compare against.

Whilst we can estimate the **posterior probability** of a structure \(P\(G\|O\)\), this is not always correctly calibrated, and often many structures with similar probabilities occur. Given the uncertainty of any learnt structures due to the large space of possible **DAGs**, and the significant effect this can have on simulated interventions, it was determined we needed a method of finding the expected performance of our learnt structures to answer a number of questions:

1. What are the acceptable properties of a dataset to employ the use of Bayesian networks?
2. What can improve or degrade the performance of various structure learning algorithms, w.r.t given properties of a dataset?
3. How does the performance degrade across different structure learning algorithms i.e. does recall suffer first, or precision, or both in tandem?
4. What effect does performance degradation of the structures have on the simulated interventions?

With these answers in mind, the aim of a synthetic evaluation tool would be: **any user wishing to employ Bayesian networks can determine if their use is suitable given a particular dataset.**

## How did we create an evaluation tool?

Many simulated datasets with accompanying true structures have been generated, attempting to exhaust the properties of Bayesian networks as much as possible. 

Following this, a select few structure learning algorithms have been benchmarked to determine their **expected performance**, with the possibility to trivially integrate new algorithms. This *attempted* exhaustion of the different possible properties of networks allows the estimation of performance on a given dataset, for a given structure learning algorithm (**Question 1**). 

Once an estimated performance has been given, if the user is not satisfied with the performance levels, they may use the tool to see how performance improvements could be made (**Question 2**). 

It may also be the case that different structure learning algorithms perform differently dependent on the properties of a dataset (e.g. less performance degradation on lower sample sizes), and this benchmarking will effectively display this (**Question 3**).
Lastly, the intervention performance has been captured, and a clear link between correct structures and interventional performance is shown (**Question 4**).
