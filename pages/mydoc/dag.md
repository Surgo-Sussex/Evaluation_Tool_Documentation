---
title: "Directed Acyclic Graph"
tags: [Graph, Network, DAG]
summary: "This page will explain the concept of a DAG, and how it relates to interventional accuracy"
sidebar: mydoc_sidebar
permalink: dag.html
folder: mydoc
---

## Directed Acyclic Graph (DAG)

A directed acyclic graph is a special case of a [mathematical graph](http://mathworld.wolfram.com/Graph.html), a collection of nodes and edges, with the following conditions enforced:

1. All edges are directed
2. The graph contains no cycles

{% include image.html file="dag.svg" alt="" caption="An example of a Directed Acyclic Graph with alphabetical variable names" max-width="800"%}

In the case of bayesian networks, a DAG has the interpretation of being a **causal** graph, where nodes correspond to variables, and a directed edge between nodes asserting significant statistical dependence, and the existance of a causal relationship under the assumption of [causal sufficiency](http://mlg.eng.cam.ac.uk/zoubin/SALD/Intro-Causal.pdf).

A DAG encodes a set of conditional independence relationships, where connected nodes are dependent and disconnected nodes are (conditionally) independent. Thus, DAGs have a probabilistic interpretation, where the joint distribution can be factorized into the product of conditional distributions of each node conditioned on their parent\(s\).


{% include image.html file="factorizedPr.svg" alt="Surface Plots" caption="" max-width="800" %}

However, the reverse is not true. Given a set of conditional independence relationships we **cannot** asertain the DAG it was generated from. This is due to **markov equivelence**, where the same set of conditional independence relationships can encode multiple DAGs; this set of DAGs is known as the **equivelence class**. For further reading refer to the [V-structure page](v_structures.html).



## Terminology

### Parent

A variable is a parent of another if it is the cause of it in the DAG

### Child

A variable is a child of another if it is caused by it.

{% include image.html file="parent_child.svg" alt="" caption="A diagram showing the parent-child relationship between two variables; A and B." max-width="800"%}

### Markov Blanket

The markov blanket of a node is: it's children, parents, and children of parents. The markov blanket is significant as the node is independent of all other nodes when conditioned on it's markov blanket. This is known as the **Markov property**.

## Neccesity of a DAG

When measuring the causal effect of a treatment on an outcome, confounding bias must be controlled for. To do this we must know what variables are confounding between our treatment and outcome i.e. what variables are parents of both the treament and outcome variable. The DAG structure encodes this directly, and it allows us to treat any variable in the graph as a treatment variable, and measure its effect on any other variable. 

If the DAG is learnt incorrectly this may cause some confounding bias to not be accounted for, or to be falsely accounted for, introducing noise into the measured causal effect. 