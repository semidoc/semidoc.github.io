---
layout: post
title: Color Coding
comments: false
redirect_from: "/2016/12/07/color_coding/"
permalink: color-coding
---

There is a very simple $$O^*(2^n)$$ (the $$O^*$$ notation suppresses polynomial factors) dynamique programming algorithm from the 70ties by Micheal Held and Richard Karp that solves the hamiltonian cycle problem. It was for a long time an open problem if one could solve the k-path problem in $$O^*(2^k)$$. There now exists algorithms that actually beat this running time (\[[Williams2009](https://arxiv.org/abs/0807.3026)\],\[[Bjorklund2010](https://arxiv.org/abs/1007.1161)\]).

I want to focus here on a technique called Color Coding developed by Alon et al. \[[Alon94](http://www.math.tau.ac.il/~nogaa/PDFS/col5.pdf)\] in '94. It was the first time that the k-path problem was solved in $$O^*(c^k)$$ for some constant $$c$$. The technique is simple but very beautiful and can also be applied to other subgraph problems.

There are two phases to a color coding algorithm. In the first phase the graph gets randomly colored with a certain number of colors. With a non-zero probability the desired subgraph is colored in a certain manner. Dynamic programming is then used to find the *well-colored* subgraph.

In the case of k-path, we first color our graph $$G$$ randomly with k colors (notice that it is not necessarily a proper coloring). A k-path (here a k-path is a path on k vertices) of our original graph is said to be *well-colored* if it uses all the k colors. If $$G$$ contains a k-path, it gets *well-colored* with probability $$\frac{k!}{k^k} > \frac{1}{e^k}$$. If $$G$$ does not contain a k-path, then it does not contain a *well-colored* k-path. Suppose we obtain a $$f(n,k)$$-algorithm for finding a *well-colored* k-path in $$G$$, this implies a $$e^kf(n,k)$$ Monte-Carlo algorithm for k-path. By repeating this algorithm a constant number of times we can make the failure probability as small as possible. The problem therefore reduces to finding a *well-colored* k-path in $$G$$.

For this goal the authors used dynamic programming. Let $$v \in V$$ and $$S \subseteq 2^k$$ and define Path$$(v,S)$$ to be $$True$$ if there exists a *well-colored* path of length $$\vert S\vert $$ ending in $$v$$. The recurrence can then be stated as:

$$
\begin{alignat*}{2}
	&\text{Path}(v, \{c_i\}) &&= True \text{ if } c(v) = c_i\  \ False \text{ o/w} \\
	&\text{Path}(v,S) &&= \bigvee_{u \in N(v)} \text{Path}(u, S \setminus \{c(v)\}) \text{ if } c(v) \in S \ \ False \text{ o/w}
\end{alignat*}
$$

The complexity of the dynamic programming algorithm is $$O(2^kn^2)$$. One therefore obtains a Monte-Carlo-algorithm for k-path running in $$O((2e)^kn^2)$$.

The same can be applied to k-trees. The only thing that has to change is the dynamic programming algorithm. For this problem we are given $$G$$ and a tree $$T$$ of size k and the question is if $$G$$ contains $$T$$ as a subgraph (not necessarily induced). We will first root our tree $$T$$ at some arbitrary vertex $$r$$. This gives a unique orientation to every edge. We will denote by $$T^i_k$$ the subtree rooted at $$i$$ with the first $$k$$ children and $$t^i_k$$ the $$k^{th}$$-child of $$i$$. We denote by $$d(i)$$ the number of children of $$i$$ in the rooted tree $$T$$. Let Tree$$(v,S,j,l)$$ be $$True$$ if there exists a copy of $$T^j_l$$ rooted at $$v$$ in $$G$$ using the colors of $$S$$. In particular this implies $$\vert S\vert = \vert T^j_k\vert$$. The recurrence now becomes:

$$
\begin{alignat*}{2}
	&\text{Tree}(v, \{c_i\}, j, 0) &&= True \text{ if } c(v) = c_i\  \ False \text{ o/w} \\
	&\text{Tree}(v,S, j, l) &&= \bigvee_{\substack{u \in N(v)\\ S' \subseteq S}} \text{Tree}(v, S',j,l-1) \wedge \text{Tree}(u,S\setminus S',t^j_l, d(t^j_l))
\end{alignat*}
$$

As one has to iterate over all the subsets of $$S$$ for each state the running time of the dynamic programming algorithm is $$O(k^22^{2k}n^2)$$ in this case.
It should be noted that the above technique can be derandomized by constructing a family of functions $$\mathcal{F}$$ from  $$[n]$$ to $$[k]$$ , such that for every $$S \in { {[n]}\choose{k} }$$ there exists a fucntion $$f \in \mathcal{F}$$ that is injective on $$S$$.

There is a very nice chapter about randomized methods like the above in the book *Parameterized Algorithms* by Cygan et al. \[[Cygan2015](http://parameterized-algorithms.mimuw.edu.pl/)\] from which this post is inspired. There is a free version available online if you are interested.
