---
title: "Metrics"
tags: [metrics, precision, recall, F1]
summary: "Metrics used in the tool"
sidebar: mydoc_sidebar
permalink: metrics.html
folder: mydoc
---

In the performance estimation tool, we use a set of metrics which we believe effectively capture the performance of various structure learning algorithms. These metrics will now be explained and justified.

## Precision

Precision, also known as the positive predictive value, is the measure of; out of the positive **predictions** made, what proportion of these were **true**. The upper bound of precision is **one**, which corresponds to when all predicted true cases are correct. The lower bound is **zero**, which indicates that none of the predicted cases were correct.

### Skeleton

In respect to the Skeleton, the precision is the proportion of learnt edges which are present in the true skeleton. 

### V-Structures

The precision w.r.t. V-Structures is the proportion of learnt V-Structures which are present in the true CPDAG.

## Recall

Recall is another binary classification metric, and is the proportion of **predicted** positive cases over **true** positive cases. The upper bound of recall is **one**, corresponding to all true positive values being predicted. The lower bound is **zero** indicating none of the true positive values have been captured.

### Skeleton

Recall of the skeleton is the proportion of the **true** undirected edges that are present in the **predicted** case.

### V-Structures

Recall of V-Structures is the proportion of the **true** V-Structures that are present in the **predicted** case.

## F1

The F1 Score is the harmonic mean of precision and recall. The harmonic mean is used as to ensure precision and recall recieve equal representation in the metric. The upper bound of this metric is one, and this is the case when both precision and recall are one - in the case of a Skeleton or V-Structure this represents perfect performance. The lower bound is zero, when both precision and recall are zero.

## Percentage of Correct Odds Ratios

The metric we use to represent interventional performance is the proportion of odds ratios that are 'correct'. To calculate this we select a target variable, and use all other variables as treatment variables. We intervene on each treatment variable to determine whether they have a protective, detrimental or no effect on the target. The 'Percentage of Correct Odds Ratios' is simply the proportion of protective or detrimental true effects which were correct asertained by the learnt model.