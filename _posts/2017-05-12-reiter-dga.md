---
layout: post
title: Logic and distributed automata 
redirect_from: "/2017/05/12/reiter-dga/"
permalink: reiter-dga
---

Hi everyone, 

today I'd like to give a glimpse of the PhD seminar [Fabian Reiter](https://www.irif.fr/users/reiterf/index) gave fairly recently. It is about the paper *Asynchronous Distributed Automata: A Characterization of the Modal Mu-Fragment* ([arxiv version](https://arxiv.org/abs/1611.08554)). 
Between the seminar and today, the paper has been accepted at [ICALP 2017](http://icalp17.mimuw.edu.pl/), so if you are at Warsaw in mid-July, you can hear about this directly from him! For any comment on this post: feuilloley [at] irif.fr.

Actually I will not talk about this paper in particular, as I am far from being an expert, but I would like to give a feeling of the topic, from the point of view of an outsider. 

Automata and logic are usually taught in different courses, and may be thought as unrelated subjects by those who are not working in these domains. It is far from being the case, and there are strong connexions between the two topics. One area of TCS for which these connexions are essential is [model checking](https://en.wikipedia.org/wiki/Model_checking). In this domain, one would like to check if some structure satisfies some property, expressed as a logical formula. In this case, a common technique is to translate the formula into an automaton. Then one can, for example, compute some product of the structure with the automaton, and the checking boils down to a reachability problem. 

To give an intuition of the logic we are talking about, let's consider the case of words over the alphabet {$a,b$}. It is easier to consider a word as a directed path, for example the word $aabab$ is represented by the following path. 

![](assets/mot2.svg){: .center-image }

A property one would like to express is, for example, ''there are no two consecutive $b$'' in the word. To do so the logic has three predicates, $\rightarrow$, $a()$ and $b()$. Given two variables $x$ and $y$ that indicate positions in the path, $x \rightarrow y$ means that $y$ is the position just after $x$ in the word. If the letter at position $x$ is $a$ (resp. $b$), then $a(x)$ (resp. $b(x)$) holds. Then the ''not-2-b'' property can be expressed as: 

$$\not \exists x,y: b(x) \wedge b(y) \wedge x \rightarrow y $$ 

This a basically first-order logic, with the predicates suited to talk about words. Note that quantifiers “talk about” single nodes. If in addition we add quantification over sets of nodes, we get [monadic second-order (MSO) logic](https://en.wikipedia.org/wiki/Monadic_second-order_logic) on words. And here comes the magic: the properties of words that are expressible with MSO are precisely the ones that corresponds to regular languages, that is languages recognized by finite automata. 

This all goes back to the 60s, and then a lot of work has been done to create bridges between special kinds of logic and types of languages. An important example is the work on trees with [tree automata](https://en.wikipedia.org/wiki/Tree_automaton). What Fabian and a few others are interested in, is the case of graphs. 

There are several kinds of logic that can be considered on graphs, and I will not dive into it. Instead I will focus on the other side: the automaton. It is not clear what adaptation of a finite automaton is the most relevant for graphs, and several such tool have been proposed. The approach of Fabian and others is to consider _distributed graph automaton_. The word _distributed_ comes from the fact that instead of having one machine considering the parts of the graph in some order, as one would do to generalize results on words and trees, here each node of the graph is equipped with an automaton. These automata take steps, and at each step they take into account their state, and the states of their neighbours, and update their own state.[^1]

The information available from the neighbours, the synchrony or not of the automata, the termination criterion, and the non-determinism allowed have a great influence on the corresponding logic. A previous paper of Fabian[^2] described a model of synchronized distributed graph automata that corresponds to the MSO logic on graphs. The current paper considers asynchronous automata, and links it with a fragment of the [modal $\mu$-calculus](https://en.wikipedia.org/wiki/Modal_%CE%BC-calculus).

For more on the topic, I let you read Fabian's papers! 

---

[^1]:For those of you who have met the LOCAL model at some point, this must sound very familiar as it is basically the same state of mind as the locality-oriented distributed computing.
[^2]:The paper _Distributed Graph Automata_ ([arxiv version](https://arxiv.org/abs/1408.3030)). The slides are also available [here](https://www.irif.fr/_media/users/reiterf/lics2015.pdf).
