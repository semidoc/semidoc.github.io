---
layout: post
title: Bootstrap Percolation
comments: false
redirect_from: "/2016/12/07/bootstrap_percolation/"
permalink: boostrap-percolation
---

Imagine a disease that spreads as follows: You become infected if you know two people that are contaminated. We can model a group of people by a graph $$G$$. People being vertices and relationships being edges. One can now ask the question: What is the minimum number of infected people in the beginning, so that everybody becomes contaminated? We are assuming that a person remains contaminated once infected. The corresponding decision problem is $$NP$$-complete in general graphs and is therefore unlikely to have an easy and compact answer.

What about simpler graphs such as grids? In multidimensional grids the problem has been introduced by statisical physics as *bootstrap percolation* and has a large literature. Let us look at the planar $$ n \times n $$ grid. Here it is quite easy to find a set of $$n$$ vertices that will eventually cover the graph. A diagonal is sufficient. The following theorem, which is part of the folklore, shows that $$n$$ is all that we can hope for. Let the cover number be the minimum number of *diseased* vertices that one needs in order to *contaminate* the whole graph. For convenience and historic reasons we will be seeing the grid as board.

**Theorem.** The cover number of the $$n \times n$$ grid is $$n$$.

*Proof.* Contaminating the diagonal of the board proves the upper bound. For the lower bound we make use of the *invariant method*: the perimeter of the contaminated part of the board can never decrease when we apply the 2-neighbour rule. As the perimeter of the board needs to be $$4n$$ in the end, we need at least $$n$$ contaminated squares in the beginning. $$\square$$

I would have probably looked for this kind of argument for a very very long time. Erdős might have said that this proof is from the *book*.
