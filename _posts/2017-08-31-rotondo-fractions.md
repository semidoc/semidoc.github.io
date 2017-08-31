---
layout: post
title: Combinatorics of continued fractions 
redirect_from: "/2017/06/19/rotondo-fractions/"
permalink: rotondo-fractions
---

Hello, the first post of this new academic year is by [Pablo Rotondo](https://www.irif.fr/users/rotondo/index) 
who gave a talk at the seminar a few months ago. As there is more to say about 
the subject, this post will probably have a follow-up later.

-----

In this post we discuss 
**[continued fraction expansions](https://en.wikipedia.org/wiki/Continued_fraction)**, 
and their statistics.

# Introduction
Continued fractions are rather ubiquitous, they pop up in various contexts such as: 
the [Euclidean Algorithm](https://en.wikipedia.org/wiki/Euclidean_algorithm), 
[Pell's equation](https://en.wikipedia.org/wiki/Pell%27s_equation) (see 
[here](http://www-bcf.usc.edu/~lototsky/PiMuEp/Pell-IMO.pdf) and 
[here](http://math.stanford.edu/~jbooher/expos/continued_fractions.pdf)) and 
[Diophantine approximation](https://en.wikipedia.org/wiki/Diophantine_approximation), the approximation of reals by rationals.

### Relation to the Euclidean algorithm and definitions.

Let us briefly see their relation to the Euclidean algorithm as an introduction.

The Euclidean algorithm allows us to calculate the greatest common divisor ($\gcd$) of 
a pair of positive integers $(x,y)$, by performing successive divisions. Supposing
$x\geq y$, the algorithm proceeds by applying $(x,y)\mapsto (y,x\bmod y)$ until the
smallest entry $y$ is $0$ and the result is then $x$. Its correctness is seen from the fact
that $x\bmod y = x - y \, \lfloor x/y\rfloor$, and therefore $\gcd(x,y)=\gcd(y,x\bmod y)$.

Let us get to the continued fractions. Given a pair of positive integers $(x,y)$
with $x\geq y$ we may write

$$
\frac{y}{x} = \frac{1}{x/y} = \cfrac{1}{\lfloor x/y \rfloor + \cfrac{x\bmod y}{y} }\,,
$$

and then continue the procedure with $\cfrac{x\bmod y}{y}$ provided that $x\bmod y \neq 0$. This is
exactly the Euclidean algorithm, and observe that the quotients $\lfloor x/y\rfloor$
that we discard in the algorithm here appear as coefficients.

Thus the Euclidean produces a continued fraction expansion 

$$
\frac{y}{x} = \cfrac{1}{m_1 + \cfrac{1}{\ddots + \cfrac{1}{m_k} }}\,,
$$

where the positive integers $m_1,\ldots,m_k$ are the successive quotients in the algorithm.

More generally, we call any such expansion a continued fraction expansion and even given infinitely many coefficients $m_1,\ldots,m_k,\ldots$ we define

$$
\cfrac{1}{m_1 + \cfrac{1}{m_2+\ddots} } = \lim_{k\to\infty}\cfrac{1}{m_1 + \cfrac{1}{\ddots + \cfrac{1}{m_k} }}\,,
$$

whose convergence we do not prove here, but you can find a proof in Kinchin's book 
(see the reference at the end of the post).

Rational numbers have exactly two expansions:

$$
\frac{y}{x} = \cfrac{1}{m_1 + \cfrac{1}{\ddots + \cfrac{1}{m_k} }}= \cfrac{1}{m_1 + \cfrac{1}{\ddots + \cfrac{1}{(m_k-1) + \cfrac{1}{1}} }}\,,
$$

while irrational numbers $\alpha\in (0,1)$ have a unique (infinite) expansion, 
which can be found applying the same Euclidean algorithm (!) to the pair $(1/\alpha,1)$.

Some remarkable examples  (for a proof see [this](https://arxiv.org/abs/math/0601660) 
arXiv note or [this](https://topologicalmusings.wordpress.com/2008/08/04/continued-fraction-for-e/))

$$
\frac{\sqrt{5}-1}{2} = \cfrac{1}{1+\cfrac{1}{1+\cfrac{1}{\ddots}}}\,,
$$

$$
\qquad e-2 = \cfrac{1}{1+\cfrac{1}{2+\cfrac{1}{1+\cfrac{1}{1+\cfrac{1}{4+\cfrac{1}{1+\ddots}}}}}}\,,
$$

the last one being made up from the concatenation of the trios $1,2n,1$ for $n=1,2,\ldots$.

### Notations 
Given an irrational number $\alpha \in (0,1)$ with continued fraction expansion

$$
\alpha  = \cfrac{1}{m_1 + \cfrac{1}{m_2+\ddots} }\,,
$$

the coefficients $m_1,m_2,\ldots \geq 1$ are called _partial quotients_ or _digits_, while the partial expansions

$$
\frac{p_k(\alpha)}{q_k(\alpha)} := \cfrac{1}{m_1 + \cfrac{1}{\ddots + \cfrac{1}{m_k}}}\,,
$$

with 
$$
\gcd(p_k(\alpha),q_k(\alpha)) = 1
$$, 
are known as *convergents*. By convention we define $p_0=0$ and $q_0=1$ for every irrational $\alpha\in (0,1)$. The denominators $(q_k(\alpha))_{k\in \mathbb{N}}$ of the convergents are known as the *continuants* of $\alpha$.

Of course, all of these definition can be extended to rational numbers by considering only the meaningful part ($k\leq$ length of the expansion). See [here](http://stackoverflow.com/questions/5440688/the-guess-the-number-game-for-arbitrary-rational-numbers) for a particularly interesting application to a game of guessing a rational number!


### Number of steps in the Euclidean algorithm.

The depth of the continued fraction expansion of a rational number $x/y$ is exactly the number of steps $\ell:=\ell(x,y)$ performed by the Euclidean algorithm on $(x,y)$. To bound $\ell$, we remark that the denominators satisfy the following recurrence relation

$$
q_{k+1} = m_{k+1}q_k + q_{k-1}\,,\quad q_0=1,q_{-1}=0\,.
$$

This is proved by induction, along with the analogous (up to the initial condition) recurrence relation for the numerators $p_k$. 
It is evident from the recurrence relation that $q_k$ is strictly increasing, furthermore

$$
q_{k+1} = m_{k+1}q_k + q_{k-1} \geq 2 q_{k-1}\,,
$$

so that $q_k \geq 2^{\frac{k-1}{2}}$.

Picking $k=\ell(x,y)$, the previous inequality reads $y\geq 2^{\frac{\ell-1}{2}}$ so that 

$$ 
\ell(x,y) \leq 1 + 2 \log_2 y\,. 
$$


# Classical Continued Fraction Statistics
We have just seen that convergents $q_k(\alpha)$ grow at least exponentially

$$
2^{\frac{k-1}{2}} \leq q_k(\alpha)\,,
$$

we briefly mention that much more precise results are known when we allow $\alpha$ to be almost any irrational in $(0,1)$.

**Theorem** [Lévy]
For almost every $\alpha\in (0,1)$ (w.r.t the Lebesgue -uniform- measure) we have

$$
\lim_{k\to\infty}\frac{1}{k}\log q_k(\alpha) = \frac{\pi^2}{12 \log 2}\,.
$$


This result is proved by using Ergodic Theory. Continued fractions are naturally associated to the
so called Euclidean Dynamical System arising from the Gauss map

$$
T \colon (0,1)\to (0,1) \,,\qquad x\mapsto \left\{\tfrac{1}{x}\right\}\,,
$$

where $\{x\}:=x-\lfloor x\rfloor$ is the _fractional part_ of $x$.

Interestingly, it is also true that the frequency of $m_j=k$ tends to
$\log_2\left(1+\frac{1}{k(k+2)}\right)$ almost surely as we take more and more quotients.
For more on this read the notes 
[here](http://www.maths.manchester.ac.uk/~cwalkden/ergodic-theory/ergodic_theory.pdf) 
or in Khinchin's book.

# Best approximations
Convergents are particularly important due to the fact that they provide the ``best'' approximations to numbers in the following sense

_Definition_ [Best approximations of the second kind]
Let $\alpha \in (0,1)$ be a real number. Then the fraction $\tfrac{p}{q}$ with $q>0$ is said to be a best approximation of the second kind for $\alpha$ if and only if

$$
|q \alpha - p| < |q^\prime \alpha - p^\prime|
$$

for any other fraction $\tfrac{p^\prime}{q^\prime}$ with $0<
q \prime \leq q$.

Remark that 
$$
|q \alpha - p| = \frac{| \alpha - p / q|}{1/q}
$$ so that this ``objective function'' can be thought of as the error of approximating $\alpha$ by $p/q$, relative to the size of the denominator $q$. We are ready to state the relation with continued fractions

**Theorem** [See Khinchin 97]
Best approximations of the second kind are necessarily convergents, and conversely, every convergent is a best approximation of the second kind, with exception of the trivial case of

$$
\alpha = \frac{1}{2},\qquad \frac{p_0}{q_0} = 0\,.
$$

Concretely, this means that if we may choose from the set of fractions whose denominators are at most $n$, the best approximation is given by $\frac{p_k(\alpha)}{q_k(\alpha)}$ with $k:= k(\alpha,n)$ is the only index $k$ for which $q_k(\alpha)\leq n < q_{k+1}(\alpha)$.


We remark that it is the continuants under the condition $q_k(\alpha)\leq n < q_{k+1}(\alpha)$ and $n\to \infty$ that will eventually interest us, rather than the classical fixed depth $k\to \infty$.

# References.

* Aleksandr Ya. Khinchin. Continued Fractions.  _New York: Dover Publications_, 1997.
* M. Einsiedler and T. Ward. Ergodic Theory: with a view towards Number Theory. _Graduate Texts
in Mathematics. Springer, 2010._
* N. P. Fogg , V. Berthé, S. Ferenczi , C. Mauduit and Anne Siegel: Substitutions in Dynamics, Arithmetics, and Combinatorics _Lecture Notes in Mathematics, Vol. 1794._
