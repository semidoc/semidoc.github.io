---
layout: post
title: Exploring, evacuating, and finding treasures, without a map
redirect_from: "/2017/06/29/exploration/"
permalink: exploration
---

Hi there! I have come back from the 
[Sirocco conference](https://sirocco2017.lif.univ-mrs.fr/) a few days ago, 
and there I saw a couple of talks on fun and easy-to-explain topics, so here is 
a post about it. (There is a review of Sirocco by Christian Konrad,
[here](www.christiankonrad.de/publications/sirocco-review-2017.pdf)). 

# Cow path problem 
You may know the following problem, or a variant of it:

A cow is looking for a treasure, 
that is located at an unknown position of a path. 
The cow cannot see very well, so it can only detect the treasure if it is just below its eyes.
The question is: what is a good strategy to find the treasure while
walking as little as possible? More precisely, if there are $d$ steps from the 
starting point to the treasure (and $d$ is unknown), is it possible to find it after $O(d)$ steps?[^1] 

I will explain the solution in the next paragraph, but I guess it is better to find it by 
yourself, if you do not know it already (it is not super tricky). 

For sure, the cow cannot go straight in one direction, as 
the treasure may be in the other direction. But when it turns back, it may be the case 
that the direction was good, and that the treasure is just a few steps further. 
The solution is basically to do a first step in one arbitrary direction, say the right, 
then to go back to the starting point, to do two steps to the left, 
to go back, do four steps to the right etc. doubling the distance every time. 

The cow will eventually find the treasure, as it goes arbitrarily far in both directions.
As for the distance travelled, the last movement from the starting point to the treasure is 
basically of the same order of magnitude as the whole distance walked from the very 
start of the procedure. This kind of exponential increase is often used in 
algorithms, see for example 
the [exponential search](https://en.wikipedia.org/wiki/Exponential_search). 
Another example is the case when one 
has an efficient algorithm for a task, but this algorithm requires some upper 
bound on a parameter of the input ; then one can guess 
a small value for the parameter, and then, if the  algorithm fails with this parameter, 
double this value, try again, etc.[^2]  

Note that the cow path problem is also known under the less bucolic name of 
[linear search problem](https://en.wikipedia.org/wiki/Linear_search_problem). 

# Competitive ratio
In the algorithm above, the ratio between the time it takes to find the treasure 
if you do not know where it is, and the time it takes to reach it if you know where it is, 
is constant. In analogy with [online algorithms](https://en.wikipedia.org/wiki/Online_algorithm), 
this ratio is called the *competitive ratio*, and it
is the usual way to measure how efficient a search algorithm is. The first step is 
to prove that a constant ratio strategy exists, and then to improve this constant.
For example in the cow path problem, the strategy above leads to an optimal ratio of 9, and it 
is possible to get a smaller constant with a randomized strategy.[^3]

# More roads diverged in a yellow wood...
So now what happens if the cow stands at the intersection of $m$ paths 
(the previous case being $m=2$)? It is natural to visit every path
one after the other, increasing the distance travelled. 
To minimize the competitive ratio, 
the best strategy is to fix some ordering on the path, and to visit them in the ordering
(1, 2, ..., m, 1, 2, etc.), and every time a new path is taken 
to multiply the distance by $m/(m-1)$.   

# ...and more cows were grazing
Then one can again improve the story by adding more cows (usually less than the 
number of paths, otherwise it is less funny[^4]).
Then the finishing time can be defined in several ways: 

* the simplest way is to say that the game finishes when one of the cows find the treasure,
* but maybe you want all the cows to be around the treasure. This case is usually called an *evacuation problem*, because it represents the situation in which the agents are looking for an exit, and every agent has to be evacuated.

In this last case there are two main possibilities:

* either the cows can communicate directly (in the robot metaphor, this a 
wireless communication)
* or they cannot: two cows can only communicate if they are at the same place (face-to-face communication).

The paper by Brandt et al. presented at the conference, and listed at the end of this post 
is about this last case, with lower and upper bounds on the competitive ratio. 
Another paper by Cyzyzowicz et al. was about evacuation from a disk, with 
some faulty agents.

# Finally, let's go exploring!
To finish this post, I would like to mention a similar but different kind of 
problem: graph exploration. 
We consider agents that move in an unknown graph, the following way. An agent starts at
a vertex, and can only see that there are some edges linking it to the rest of the graph.
Then it chooses such an edge, reveal a new node, and a set of outgoing edges. 
Then again it chooses an edge etc. 
At each steps the part of the graph that has been explored increases, and the goal is to explore 
the graph completely, as fast as possible. 
Once again the question is: what is the good strategy for these agents? More precisely, 
how to minimize the competitive ratio (which is defined here, as the ratio between the 
time of exploration, and the time it would take to explore the graph if the agents had a map of the graph)?
Again there are many variations. A paper on this topic at the conference is by Disser at al.

---

#### Papers and notes
* Amos Korman, Jean-Sébastien Sereni, Laurent Viennot: 
[Toward more localized local algorithms: removing assumptions concerning global knowledge](https://arxiv.org/abs/1512.03306). 
Distributed Computing 26(5-6): 289-308 (2013)
* Sebastian Brandt, Klaus-Tycho Förster, Benjamin Richner, and Roger Wattenhofer: 
[Wireless Evacuation on $m$ Rays with $k$ Searchers](https://sirocco2017.lif.univ-mrs.fr/preproceedings/Wireless%20evacuation%20on%20m%20rays%20with%20k%20searchers.pdf) SIROCCO 2017.
* Jurek Czyzowicz, Konstantinos Georgiou, Maxime Godon, Evangelos Kranakis, Danny Krizanc, Wojciech Rytter, and Michal Wlodarczyk:
[Evacuation from a Disc in the Presence of a Faulty Robot](https://sirocco2017.lif.univ-mrs.fr/preproceedings/Evacuation%20from%20a%20disc%20in%20the%20presence%20of%20a%20faulty%20robot.pdf) SIROCCO 2017.
* Yann Disser, Frank Mousset, Andreas Noever, Nemanja Skoric, and Angelika Steger:
[A General Lower Bound for Collaborative Tree Exploration](https://sirocco2017.lif.univ-mrs.fr/preproceedings/A%20general%20lower%20bound%20for%20collaborative%20tree%20exploration.pdf)

The weird section titles are references to *[a poem](http://www.bartleby.com/119/1.html)* 
and a *[comics](http://www.gocomics.com/calvinandhobbes/1995/12/31/)* 

Contact for comments: feuilloley at irif dot fr.

#### Footnotes
[^1]: There is not a great difference between the continuous and the discrete setting here. The only subtlety is that in the continuous setting, there must be a guarantee that the treasure cannot be arbitrarily close to the starting point, otherwise any first move would already be arbitrarily bad. 
[^2]:For an example in network distributed computing, see the paper by Korman et al., in which the unknown parameter is the size of the network.
[^3]:References for these results and the ones of the following section can be found in the paper by Brandt et al. 
[^4]:If you think this story is becoming too silly, just replace the word 'cow' by 'robot' or 'agent', and 'treasure' by 'target'.
