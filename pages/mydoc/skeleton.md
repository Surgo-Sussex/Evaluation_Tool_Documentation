---
title: "Skeleton"
tags: [skeleton]
summary: "Skeleton of a Bayesian Network"
sidebar: mydoc_sidebar
permalink: skeleton.html
folder: mydoc
---

The skeleton of a structure is an **undirected** graph where edges suggest some relationship between variables, but do not assert the **direction** of this relationship.

{% include image.html file="dag2skel.svg" alt="" caption="Left: An example DAG. Right: The corresponding skeleton of the DAG." max-width="800" %}

This is of significance as constraint-based structure learning algorithms, such as PC, independently learn the skeleton and direction.