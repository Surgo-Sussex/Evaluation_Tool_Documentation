---
title: "Use Case: Barley Dataset"
tags: [use_case]
keywords: Use Case
summary: "This guide will walk you through using the tool with a standard bayesian network benchmarking dataset; Barley."
sidebar: mydoc_sidebar
permalink: barley.html
folder: mydoc
---


## Introduction

In this example we will be using a dataset which has an expert determined *True* structure, the Barley dataset. As we have the *True* case avaliable to us, we can validate the tools correctness on this example.

The Barley dataset and assosiated bayesian network were constructed as a decision support system for growing malting barley without use of pesticides, more information can be found in the paper: [A decision support system for mechanical weed control in malting barley](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.144.2646&rep=rep1&type=pdf).

{% include image.html file="barley.svg" alt="Barley Network" caption="The Barley structure visualized" max-width="600" %}

## Phase I

In this phase we determine expected performance with the properties we can *easily* obtain from the dataset, these are:

* The number of samples
* The number of variables
* The maximum amount of levels

Later, we will use our preliminary bayesian network to estimate other properties, and further refine our expected performance.

### Determining the properties of the Dataset

The first two properties; number of samples and number of variables, are trivial to obtain in R:

```r
#Load the barley dataset from a CSV file
barley <- read.csv('barley.csv')
#Print the number of observations (rows in the dataframe) to the console
print(nrow(barley))
#Print the number of Variables (columns in the dataframe) to the console
print(ncol(barley))
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
#Print the output of the function, using the barley dataset, loaded earlier, as input
print(get.max.levels(barley))
````

Using these methods we determine that the barley dataset's properties are the following:

| Samples | 10,000 | 
| Variables | 48| 
| Average Levels | 3 |

### Using the tool

Once these three properties have been ascertained, we can use the tool to get a performance estimate and determine whether the usage of causal inference is appropriate. We do this by inputting the found properties into the tool's dropdown boxes:

{% include image.html file="barley_usage_1.png" alt="Barley Network" caption="Box plot performance estimate for the Barley dataset properties" max-width="600" %}

The produced box plots show the expected Skeleton F1 performance (Refer to metric section in the glossary [here](metrics.html)). Each box corresponds to one of the structure learning algorithms currently in the tool. You can change the metric to view directional, or precision/recall performance in the drop-down illustrated in the picture below:

{% include image.html file="barley_usage_2.png" alt="Barley Network" caption="Changing the metrics for the box plots" max-width="600" %}

The tool estimates with barley's properties we should have adequate Skeleton and V-structure performance. 

At this point we would then use the *best performing algorithm* to create a preliminary structure. After which the process outlined in the main section will be continued until phase II.

{% include image.html file="barley_learnt.png" alt="Barley Network" caption="A learnt barley network using PC+TABU" max-width="600" %}

## Phase II

In this phase, the learnt structure from phase I and the dataset are used to estimate the imbalance. We do this as imbalance can have a severe effect on performance.

To estimate the imbalance, we will be making use of a function from the **bnlearn** package, **alpha.star**. This algorithm requires a structure, and assosiated dataset and produces a BDE ISS (Alpha), which corresponds to the imbalance in the estimation tool.

```r
#Learn a prelimanary structure
barley.structure <- bnlearn::pc(barley)

#extend the CPDAG learnt from PC to a DAG
barley.structure <- bnlearn::cextend(barley.structure)

#Estimate the Imbalance
print(bnlearn::alpha.star(barley.structure, barley))
````

In the case of barley, we find the data is **extremely** imbalanced, with an estimated alpha of 0.08. We can add this information to the tool to adjust our estimation:

{% include image.html file="barley_phase_II.png" alt="Barley Network" caption="A revised estimation of barley performance" max-width="600" %}

From this updated estimation, we can see that expected performance is very poor - and it is not reccomended to proceed with any of the structure learning algorithms currently benchmarked in this tool.

## Surface Plots

The surface plot section of the tool can be used to investigate the variable/sample space of the expected performance of the algorithms, to determine if there are any adjustments a user could make to the dataset to improve performance.

{% include image.html file="barley_surface.png" alt="Barley Network" caption="A revised estimation of barley performance" max-width="600" %}

