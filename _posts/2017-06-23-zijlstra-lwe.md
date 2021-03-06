---
layout: post
title: Learning with Errors- based Cryptography
redirect_from: "/2017/06/23/zijlstra-lwe/"
permalink: zijlstra-lwe
---

Hi! The current blog post has been written by **Timo Zijlstra**, who is doing an internship at the IRIF, and gave a talk at the seminar a few weeks ago.

---

Cryptographic primitives such as encryption and signature schemes rely on computationally hard problems. A good scheme is constructed in such a way that any method that breaks the scheme automatically yields an algorithm that solves the underlying hard problem. The [RSA encryption scheme](https://en.wikipedia.org/wiki/RSA_(cryptosystem)), for example, relies on the fact that there is no known algorithm that [efficiently factorizes large integers](https://en.wikipedia.org/wiki/Integer_factorization). If someone would be able to break RSA, then this would imply a method to factorize large integers. This would be a major breakthrough in mathematics, since the factorization problem has been intensively studied. Therefore, it is safe to assume that RSA cannot be broken. Most of implemented cryptography relies on the hardness of the factorization problem (RSA) or the [discrete logarithm problem](https://en.wikipedia.org/wiki/Discrete_logarithm) ([Elliptic Curve Cryptography](https://en.wikipedia.org/wiki/Elliptic_curve_cryptography)). However, [Shor's quantum algorithm](https://en.wikipedia.org/wiki/Shor%27s_algorithm) can be applied to both of these problems, making the cryptosystems unsafe against quantum adversaries. 


This is the reason why we need to find mathematical problems that are computationally hard even for quantum computers. The best candidates for use in post-quantum cryptography are [lattice problems](https://en.wikipedia.org/wiki/Lattice_problem) such as the [Shortest Vector Problem](https://en.wikipedia.org/wiki/Lattice_problem#Shortest_vector_problem_.28SVP.29) (SVP) and the [Closest Vector Problem](https://en.wikipedia.org/wiki/Lattice_problem#Closest_vector_problem_.28CVP.29) (CVP). There is currently no known (quantum) algorithm that solves these problems efficiently. 

One lattice problem that is particularly adaptable for cryptographic applications is the **Learning With Errors** (LWE) problem. It consists of solving a system of approximate linear equations over the integers modulo some prime $q$. Let 
$$
\textbf{s} \in \mathbb{Z}_q^n
$$ 
be a secret vector and 
$$
\textbf{a}^{(1)}, \dots, \textbf{a}^{(m)}
$$ 
vectors taken from the uniformly random distribution over 
$$
\mathbb{Z}_q^n
$$
. Given the system of approximate equations:

$$
\begin{align*}
\textbf{a}^{(1)} \cdot \textbf{s} &\approx b_1 \\
&\vdots \\
\textbf{a}^{(m)} \cdot \textbf{s} &\approx b_m,
\end{align*}
$$

where the approximation is 'close' in a sense that will be made clear later, the LWE-problem is to find 
$$\textbf{s}$$. 
Note that if the approximations were equalities, then it would be easy to solve the system using [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination). This method does not work for LWE though, because we cannot obtain a new approximation by adding an arbitrary number of multiples of approximate equations. The cumulative approximation error would grow so large that there is no information left in the 'approximation' obtained by summation and scalar multiplication. It is shown by Regev[^1], that a (quantum) algorithm that solves LWE would imply a quantum algorithm that solves several believed to be hard lattice problems, among which SVP.

More formally, the LWE-distribution is defined as follows. 

**Definition**
Let 
$$\textbf{s} \in \mathbb{Z}_q^n$$ 
be a secret vector and 
$$\chi$$ 
an error distribution over 
$$\mathbb{Z}$$. 
The LWE distribution 
$$\mathcal{A}_{s, \chi}$$ 
is sampled by taking a vector 
$$\textbf{a} \leftarrow \mathbb{Z}_q^n$$ 
and an integer 
$$e \leftarrow \chi$$ 
and outputting the pair 
$$(\textbf{a}, b:= \textbf{a} \cdot \textbf{s} + e)$$. 


This distribution is pseudorandom: given $m$ independent samples from either (1) the uniform distribution, or (2) the LWE- distribution, we cannot determine which is the case. To distinguish between the uniform and the LWE- distribution is equally hard as solving the LWE-problem, that is, finding the secret vector $\textbf{s}$. This makes the construction of LWE-based semantically secure cryptosystems possible, such as Regev's encryption scheme:

* Key generation. The private key is a random vector $\textbf{s} \in \mathbb{Z}_q^n$. To create a public key, we take $m$ vectors $\textbf{a}^{(1)}, \dots, \textbf{a}^{(m)}$ independently from the uniform distribution, and $e_1, \dots, e_m$ from $\chi$. The public key is given by the $m$ LWE samples $(\textbf{a}^{(i)}, b_i)$, where $b_i = \textbf{a}^{(i)} \cdot \textbf{s} + e_i$ for $i$ from 1 to $m$. In matrix notation: $\textbf{b} = A\textbf{s} + \textbf{e}$.
* Encryption. To encrypt a bit $x$, we take a random vector $v \in \{0, 1\}^m$ and output the pair
	
$$
	(\textbf{c}_1, c_2) = \big( A\textbf{v}, \textbf{b} \cdot \textbf{v} + x \cdot \left\lfloor \frac{q}{2} \right\rfloor \big),
$$

that is, we take a subset of the row vectors and encrypt the bit in the subset sum of the samples.
		
* Decryption. Using the secret key $\textbf{s}$, compute:
$$
	\begin{align*}
	c_2 - \textbf{c}_1 \cdot \textbf{s} &= \left(A\textbf{s} + \textbf{e}\right) \cdot \textbf{v} + x \left\lfloor \frac{q}{2} \right\rfloor - (A\textbf{v})\cdot \textbf{s} 
	\\
	&= (A\textbf{s})\cdot \textbf{v} - (A\textbf{v})\cdot \textbf{s} + \textbf{e}\cdot \textbf{v} + x \left\lfloor \frac{q}{2}\right\rfloor
	\\
	&\approx  x \left\lfloor \frac{q}{2}\right\rfloor, 
	\end{align*}
$$

	since both $\textbf{e}$ and $\textbf{v}$ are vectors of small norm. If this outcome is closer to zero than to $\left\lfloor \frac{q}{2}\right\rfloor$, then the decryption of $(\textbf{c}_1, c_2)$ is $0$, else it is $1$.

---
[^1]:O. Regev. [On lattices, learning with errors, random linear codes, and cryptography](http://www.cims.nyu.edu/~regev/papers/qcrypto.pdf). J. ACM, 56(6):1–40, 2009. Preliminary version in STOC 2005. 

