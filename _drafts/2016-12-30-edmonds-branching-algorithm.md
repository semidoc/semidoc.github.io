---
layout: post
title: Edmonds Branching Algorithm
comments: false
redirect_from: "/2016/12/30/edmonds_branching_algorithm/"
permalink: edmonds-branching-algorithm
---

The Minimum Spanning Tree (MST) problem asks for a spanning tree of minimum weight given a edge weighted graph $G$.
Where [Kruskal's](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm) and [Prim's](https://en.wikipedia.org/wiki/Prim%27s_algorithm) algorithm
for the MST problem are standard in any introductory algorithms class the directed version is a less known classic.
Here one is given a directed edge weighted graph and needs to find a minimum spanning arborescence (also called a optimum branching).


We have so far only seen how to calculate the minimum branching from a particular vertex, supposing that a spanning arborescene exists at that
vertex. One can easily find a reduction so that the starting vertex does not need to be known without changing the running time of the algorithm.
We leave that to the interested reader as an exercise.
