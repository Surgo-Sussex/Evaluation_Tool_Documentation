---
title: "Imbalance"
tags: [imbalance]
summary: "What is the imbalance of a bayesian network, and how can it be estimated?"
sidebar: mydoc_sidebar
permalink: imbalance.html
folder: mydoc
---

## Imbalance explained
Variable imbalance refers to the difference between the respective probabilities of the possible values of each variable. In the context of a multinomial distribution - such as variables in a bayesian network - it can be quantified by the $\alpha$ vector of the Dirichlet distribution from which the distribution parameters are drawn.

{% include image.html file="dirichlet_plots.png" alt="" caption="Distribution of probabilities for a 3-level variable with varying $\alpha$" max-width="800" %}

$\{\alpha_{i}\}_{i=1}^{k} < 1$ produces imbalanced variables, $\{\alpha_{i}\}_{i=1}^{k} > 1$ produces balanced, and $\{\alpha_{i}\}_{i=1}^{k} = 1$ gives each ratio equal probability.


## How imbalance effects the learning of a network
Learning bayesian network structure relies upon each variable having a different distribution depending on the configuration of its parents. Whilst this means *some* imbalance is necessary for the structure to be identifiable, it also means *too much* imbalance can be an issue.

* Too balanced: For a network with too little imbalance, i.e. where the values in $\alpha$ are very large, the distribution of each variable will be roughly uniform, thus a larger amount of data will be required for any observed difference in distributions to be statistically significant.

* Too imbalanced: Similarly, for networks which are too imbalanced \(small $\alpha$\), it becomes likely variables' distributions will be similar for different parent configurations, again requiring more data.


## How to estimate imbalance
