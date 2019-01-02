---
title: "V-Structures"
tags: [V-Structures]
summary: "V-Structures of a Bayesian Network"
sidebar: mydoc_sidebar
permalink: v_structures.html
folder: mydoc
---

## Markov Equivelence

Mentioned in the [DAG](dag.html) section, markov equivelence is the property that a set of conditional independence relationships can encode multiple DAGs. This set of DAGs is known as the eqiuivelence class, and can be depicted by a single graph known as a Complete Partially Directed Acyclic Graph, or CPDAG.

{% include image.html file="cpdag.svg" alt="" caption="The CPDAG of the example DAG given <a href='/dag.html'>here</a>. Notice some edges are undirected." max-width="800" %}

There are three cases where direction can be enforced:

1. A triplet of nodes \{A, B, C} is a V-Structure (Explained below).
2. The alternative direction would create a cycle.
3. The alternative direction would create a V-Structure.

## V-Structure

A V-structure is a triplet of nodes \{A, B, C\} where two of the nodes are directed towards the other, with no link between them e.g. A → B ← C.
This can be directed as there is only one set of conditional independence relations which can produce this triplet: A !⊥ B, C!⊥B and A⊥B. Thus, the importance of V-Structures is highlighted: In any structure learning algorithm which conforms to the faithfulness assumption; we can only learn up to the CPDAG.
