---
title: "Use Case: ALARM Dataset"
tags: [use_case]
keywords: Use Case
summary: "This guide will walk you through using the tool with a standard bayesian network benchmarking dataset; ALARM."
sidebar: mydoc_sidebar
permalink: alarm.html
folder: mydoc
---

## Introduction

The ALARM ("A Logical Alarm Reduction Mechanism") is an expertly constructed patient monitoring bayesian network. Futher information regarding the network can be found here: [The ALARM Monitoring System: A Case Study with two Probabilistic Inference Techniques for Belief Networks](https://link.springer.com/chapter/10.1007/978-3-642-93437-7_28)

As the network is expertly constructed, it has a **ground truth** structure which we can observe. For this reason, ALARM, among other expertly constructed networks, are often used in validating structure learning approaches.

{% include image.html file="ALARM_SVG.svg" alt="Alarm Network" caption="The ALARM structure visualized" max-width="600" %}

In this example, the tool will be use to estimate the expected performances of multiple structure learning algorithms. As we have access to the ground truth, we will then validate these estimates. Bare in mind, this is **not** a typical step, but a demonstration of the validity of the tool.

{% include note.html content="The ALARM network, as well as the corresponding data, is available in the [bnlearn package](http://www.bnlearn.com/)" %}

## Phase I

As previously explained in the [overview](index.html), the tool has 2 phases, and this use-case will first discuss phase I.

### Determining the properties of the dataset

There are **three** properties of the dataset which can be used to increase the accuracy of the estimated performance in phase I:

* The number of samples
* The number of variables
* The maximum amount of levels

The first two are trivial to obtain, and can be obtained in R in the following manner:

```r
#Load the BNLearn library to access the ALARM dataset
library(bnlearn)
#Load the alarm dataset from BNLearn
data('alarm')
#Print the number of observations (rows in the dataframe) to the console
print(nrow(alarm))
#Print the number of Variables (columns in the dataframe) to the console
print(ncol(alarm))
````
If the data was stored locally on a computer, these properties could be easily found using a tool such as Microsoft Excel.

The third property is a little more complex, as it requires the counting of all potential values each variable can take. The following function will determine this property in R:

```r
#Function to determine the maximum number of levels
get.max.levels <- function(data) {
  dist <- c()
  for(i in 1:ncol(data)) {
    dist <- c(dist, length(levels(data[,i])))
  }
  return(max(dist))
}
#Print the output of the function, using the ALARM dataset as input
print(get.max.levels(alarm))
````

Once these properties have been determined, phase I of the tool can be used to get a performance estimate. For the alarm dataset, these properties are the following:

| Samples | 20,000 | 
| Variables | 37| 
| Average Levels | 4 | 

### Using the tool

The tool has two seperate user interfaces, and whilst they provide similar information, they have different purposes.

The following are the steps to estimate the performance of the alarm network:

1. The properties of the ALARM network are input into the *Input Properties* form.
2. The algorithm is then selected, in this case PC (Constraint-Based).
3. The **Estimate** button is pressed.


{% include image.html file="alarm_usage1.png" alt="Table Results" caption="The first part of a tool, where results for each benchmarked algorithm is shown as a box-plot" max-width="800" %}

The results will then be updated, giving both Skeleton and V-Structure performance (for more information on what this means, [click here](metrics.html)) available from the drop-down. This information can then be used to to determine if the performance is *sufficient* for your requirements.

## Phase II

In this phase, the learnt structure from phase I and the dataset are used to estimate the imbalance. We do this as imbalance can have a severe effect on performance.

To estimate the imbalance, we will be making use of a function from the **bnlearn** package, **alpha.star**. This algorithm requires a structure, and assosiated dataset and produces a BDE ISS (Alpha), which corresponds to the imbalance in the estimation tool.

```r
#Learn a prelimanary structure
alarm.structure <- bnlearn::pc(alarm)

#extend the CPDAG learnt from PC to a DAG
alarm.structure <- bnlearn::cextend(alarm.structure)

#Estimate the Imbalance
print(bnlearn::alpha.star(alarm.structure, alarm))
````

In this case, we find the alpha 8.7, and we update this in our tool appropraitely. 

{% include image.html file="alarm_usage2.png" alt="Barley Network" caption="A revised estimate of our structure learning performance, making use of imbalance" max-width="600" %}

## Surface Plots

The surface plot section of the tool can be used to investigate the variable/sample space of the expected performance of the algorithms, to determine if there are any adjustments a user could make to the dataset to improve performance.

{% include image.html file="barley_surface.png" alt="Barley Network" caption="An image of the surface plot tool" max-width="600" %}


### Validating the performance

The true performance of the PC Algorithm on the ALARM dataset is the following:

| Aspect | Precision | Recall | F1 |
|-------|--------|---------|---------|
| Skeleton | 1 | 0.87 | 0.93 |
| V-Structures | 0.64 | 0.35 | 0.45 |

Which falls within the estimatation of the tool.

