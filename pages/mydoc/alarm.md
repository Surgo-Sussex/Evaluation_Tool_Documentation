---
title: "Use Case: ALARM Dataset"
tags: [getting_started]
keywords: release notes, announcements, what's new, new features
last_updated: July 16, 2016
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

As previously explained in the [overview](/index.html), the tool has 2 phases, and this use-case will first discuss phase I.

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
| Maximum Levels | 4 | 

### Using the tool

The tool has two seperate user interfaces, and whilst they provide similar information, they have different purposes.

#### Table Results

The following are the steps to estimate the performance of the alarm network:

1. The properties of the ALARM network are input into the *Input Properties* form.
2. The algorithm is then selected, in this case PC (Constraint-Based).
3. The **Estimate** button is pressed.

{% include note.html content="The tool will change the entered properties to match the closest experiments performed by the synthetic evaluation tool." %}

{% include image.html file="tool_1.png" alt="Table Results" caption="The first part of a tool, where results are presented in table format" max-width="800" %}

The results will then be updated in the table, giving both Skeleton and V-Structure performance (for more information on what this means, [click here](/skeleton.html)). This information can then be used to to determine if the performance is *sufficient* for your requirements.
If not, or if further investigation is desired, the next component of the tool, surface plots, can be used to investigate the performance space of your dataset, and help identify how performance could be improved.

#### Surface Plots

In a similar manner, the properties of the Alarm dataset are input into the Options, excluding Variables and Samples as these are the space we are exploring. So in the case of Phase I, this is only the Maximum Levels. The user can then investigate the performance of the Sample/Variable space for the properties of the ALARM network.

{% include image.html file="tool_2.png" alt="Surface Plots" caption="The second part of the tool, where the experiment data is used to create a surface plot of performance" max-width="800" %}


### Validating the performance

The true performance of the PC Algorithm on the ALARM dataset is the following:

| Aspect | Precision | Recall | F1 |
|-------|--------|---------|---------|
| Skeleton | 1 | 0.87 | 0.93 |
| V-Structures | 0.64 | 0.35 | 0.45 |

Which falls within the estimatation of the tool.

## Phase II