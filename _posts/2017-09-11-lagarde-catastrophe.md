---
layout: post
title: One-bit catastrophe
redirect_from: "/2017/09/11/lagarde-catastrophe/"
permalink: lagarde-catastrophe
---

Hi! Today [Guillaume Lagarde](https://www.irif.fr/~glagarde/) 
gives an overview of the recent preprint he has with
his advisor [Sylvain Perifel](https://www.irif.fr/~sperifel/): 
[Lempel-Ziv: a “one-bit catastrophe” but not a tragedy](https://arxiv.org/abs/1707.04312).
He presented parts of it at the PhD seminar last spring.  

## A strange scenario

Imagine you compressed a file using your favorite compression
algorithm, but you realize that there was a typo in the file. 
You then correct it, adding a single bit to the original file.

Compress it again and you get a much
larger compressed file... for just a one-bit difference! Much
compression algorithms do not have this strange behavior, but for
LZ'78, one of the most famous of them, this surprising scenario might
well happen. This is what we will overview in this post.

## Introduction to LZ'78 and notations

[Lempel-Ziv (LZ in short)
algorithms](https://en.wikipedia.org/wiki/LZ77_and_LZ78) refer to a
bunch of lossless compression techniques introduced by [Abraham
Lempel](https://en.wikipedia.org/wiki/Abraham_Lempel) and [Jacob
Ziv](https://en.wikipedia.org/wiki/Jacob_Ziv) that are used as key
ingredients in various places such as
*[deflate](https://en.wikipedia.org/wiki/DEFLATE)*,
*[gif](https://en.wikipedia.org/wiki/GIF)*,
*[gzip](https://en.wikipedia.org/wiki/Gzip)*, etc.  The version we
consider is LZ'78, and works by cutting the word $$w$$ we want to
compress into substrings, called blocks, $$w = m_1m_2\dots m_k $$,
such that each block $$m_i$$, except maybe the last one, is a maximal
extension of a previous block, that is: $$m_i = m_j.a $$ for some
letter $$a$$ and $$m_i$$ is not equal to $$m_l$$, for $$l < i$$.

For example, consider the word $$w = 00010110100001 $$. The first
block is always the first letter (it is the extension of the empty
word $$\epsilon$$), so that $$m_1 = 0$$. Then, $$m_2 = 00$$, since
this is the smallest prefix of $$0010110100001$$ which is not equal to
a word in the set $$\{\epsilon,0\}$$. Then, $$m_3 = 1$$ since this is
the smallest prefix of $$10110100001$$ which is not equal to a word in
$$\{\epsilon,0,00\}$$, and so on. In the end, $$w$$ is cut as

$$0|00|1|01|10|100|001$$


It is not hard to see that there is a unique cut that satisfies this
property. Then, the compression algorithm LZ'78 encodes each block
$$m_i$$ as a couple $$(p_i,a_i)$$ where $$p_i$$ is a *pointer* to its
*predecessor* $$m_j$$ together with the letter $$a_i$$ such that $$m_i
= m_j.a_i$$.

The previous word $$w$$ is thus encoded as
	
$$(\epsilon,0);(0,0);(\epsilon,1);(0,1);(2,0);(4,0);(1,1)$$

The compression size is the number of bits needed to represent the
compressed version of the word and is equal to $$\Theta(\sum_{i=1}^k
(|p_i| + 1))$$. In fact, it can be shown that the order of the
compression size is $$\Theta(k\log k)$$ where $$k$$ is the number of
blocks; this shows that the important and unique parameter needed is
$$k$$, the number of blocks. The *dictionnary* of a word $$w$$,
written $$D(w)$$ will be the set of all the blocks $$m_i$$. With this
notation in hand, the size of the compression can be rephrased as
$$\Theta(|D(w)| \log |D(w)|) $$. A word is said to be *incompressible*
if the size of the compression is of the order of the size of the
word. Equivalently, a word is incompressible if $$|D(w)|=
\Theta\left(\frac{|w|}{\log |w|}\right)$$.


The most compressible words are obtained by taking the concatenation
of the prefixes of any word $$x_0.x_1\dots x_{n-1}$$. That is, the
most compressible words are of the form $$w
=x_0.(x_0.x_1)(x_0.x_1.x_2)...(x_0.x_1\dots x_{n-2}.x_{n-1})$$. The
size of the dictionnary of such word is $$\Theta\left(|w|^{1/2}\right)$$.


## One-bit catastrophe and results

The *one-bit catastrophe question* asks whether there exists a
compressible (infinite) word $$w$$ such that $$0w$$ is
incompressible. We answer positively this question by showing our main
theorem:

**Theorem 1:** There exists an infinite word $$w$$ such that the
compression ratio changes from $$0$$ to at least $$\frac{1}{6075}$$ by
adding one single bit in front of $$w$$.

In fact, this result is not as tragic as it seems. Indeed, for the
ratio to change, the word $$w$$ needs to be compressible, but very
slowly; in symbol $$|D(w)| = \Omega\left(\frac{|w|}{\log^2 |w|}\right)$$ -- this
is quite close to an incompressible word!

More generally, we can be more precise by considering the possible
variations of the compression size on finite words:

**Theorem 2** For any finite word $$w\in\{0,1\}$$ and any letter $$a$$

$$|D(aw)| \leq 3\sqrt{|w|.|D(w)|}.$$

Moreover, we can show that this result is tight up to a multiplicative constant.

In particular, Theorem 2 tells us that any compressible word $$w$$ for
which the size of the dictionnary is $$o\left(\frac{|w|}{\log^2 |w|}\right)$$
remains compressible when we add any letter in front of it, i.e
$$|D(aw)| = o\left(\frac{|aw|}{\log |aw|}\right)$$.

Theorem 2 also tells us that for the most compressible words, the
variation can change from $$\Theta \left(|w|^{1/2}\right)$$ to
$$\Theta \left(|w|^{3/4}\right)$$.

## High level ideas of the proofs

The first step is to understand how to get a *weak* catastrophe, that
is a word $$w$$ such that $$|D(w)| = \Theta\left(|w|^{1/2}\right)$$ and $$|D(0w)|
= \Theta\left(|0w|^{3/4}\right)$$. For that, we consider a word $$w$$ which is
the concatenation of the prefixes of a word $$x$$, where $$x$$ has a
maximal number of different substrings: $$x$$ is a 
[de Bruijn word](https://en.wikipedia.org/wiki/De_Bruijn_sequence);
this alone should be sufficient to get a weak catastrophe (simulations
say!), but we were not able to prove it directly. Instead, we need to
add in an adaptative way small words between the prefixes, called
gadgets, in order to have more control on the parsing of $$w$$ and
$$0w$$ simultaneously.


With this *weak* catastrophe in hand, the idea is now to create the
*true catastrophe* (from compressible to incompressible word). We do
that by creating probabilistically a lot of "independent de
Bruijn-style words" which will be used to create a lot of independent
weak catastrophes at the same time. Then by tuning some parameters
like the size or the number of weak catastrophes, together coupled
with new and independent gadgets between the words, we get a
catastrophe.

## Open questions

Is it possible to push the limits as far as to get a catastrophe where the
compression ratio changes from $$0$$ to $$1$$? This would yield as the
worst possible configuration.

The main challenge, to our mind, is to remove the gadgets in our
constructions. This would mean that the *weak catastrophe* is the
typical case for optimally compressible words.
