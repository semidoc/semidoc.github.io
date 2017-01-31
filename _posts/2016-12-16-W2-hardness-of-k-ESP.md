---
layout: post
title: $W[2]$-hardness of $k$-ESP
comments: false
redirect_from: "/2016/12/16/w2_hardness_k_ESP/"
permalink: w2-hardness-k-ESP
---

This is joint work with [Christoph Dürr](http://www-desir.lip6.fr/~durrc/).

Let us first define the main problem of this post.

<div class="definition">
  <i>Eccentricity Shortest Path</i>: Given a graph $G$ and two vertices $s$ and $t$, is there a shortest $s-t$ path with eccentricity at most $k$ ?
</div>

Here the eccentricity of a path is the maximal distance from any vertex to the path. The problem has been studied and most relevant questions have been answered by
Dragan and Leitert \[[Dragan15_1](https://arxiv.org/abs/1511.05109),[Dragan15_2](http://link.springer.com/chapter/10.1007/978-3-319-21840-3_23)\]. The problem is $$NP$$-complete in the general case, but there is a dynamic programming algorithm running in $$n^{O(k)}$$ for fixed $$k$$.
A question that remained open was if the $$k$$-Excentricity Shortest Path problem ($$k$$-ESP) becomes fixed parameter tractable when parameterized by $$k$$. A fixed parameter tractable algorithm has running time
$$ f(k) n^c$$ for some function $$f$$ independent of $$n$$ and some fixed constant $$c$$. We give strong evidence that this is unlikely.

It suffices to give some reduction from some $$W[l]$$-hard problem, for some $l>0$, to $$k$$-ESP. In our case this will be the Hitting Set problem.

<div class="definition">
  <i>Hitting Set</i>: Given $m$ sets over a universe of size $n$, the goal is to select $k$ elements of the universe such that every set contains at least one selected element.
</div>

The hitting set problem is $$W[2]$$-complete. For proving fixed parameter intractibility we could use $$f(k) n^c$$ time for the reduction, but a simple polynomial reduction is sufficient in our case. Given an instance $I$ of the Hitting Set problem we construct a graph $G$ such that $G$ contains a k-ESP iff $I$ admits a hitting set of size $k$. The graph $G$ contains $k$ groups of $n$ vertices numbered 1 to $k$. The vertices of groups $i$ and $$i+1$$ are connected in a complete bipartite manner. In addition, there is a vertex $s$ connected to all vertices of group 1 and a vertex $t$ to all vertices of group $k$.
For every $S$ of the $m$ sets in $I$ we introduce a vertex $v_S$. For every element $i \in S$ there exists a path $P_{S,i}$ of length $k$ from $v_S$ to the vertex $i$ in each of the $k$ groups described above.

Now every shortest $s-t$ path has length $k+1$ and visits exactly one element of each group. Every shortest $s-t$ therefore defines a set $H$ of at most k vertices. It is not hard to verify that $H$ is a hitting set for $I$ iff the corresponding path has eccentricity at most $k$. This completes the reduction.

![Illustration of the reduction](assets/reduction-w2-large.svg )
