---
layout: default
title: The Uniqueness of the Prime 73
---

# The Uniqueness of the Prime 73

*Jul 14 2024*

I am not the only one to think *73* is special. See [73 (number)](https://en.wikipedia.org/wiki/73_(number)) for a Wiki
page on *73*. But I have my own particular reason for seeing it as special.

We start with a question. But before get there, we start with some definitions. For integers *N* larger than *1*,
consider the integers mod *N*. Or to put it another way, map integers to their remainders after dividing by *N*. The
remainder will be a number between *0* and *N - 1*. You can add and multiply these remainders and take the remainder of
the result. This creates well defined addition and multiplication mod *N*. Now, of these remainders, only take the non
zero remainders that have no common factor with *N*. For example, if *N* is *6*, then only *1* and *5* satisfy this
condition. In mathematics, these remainders under multiplication are called the multiplicative group mod *N*. It is
**group** because it has an operation, multiplication, which combines two elements of the group to produce another
element. Now in this multiplicative group, some elements have the property that if you square it, i.e. multiply it
against itself, it gives the remainder *1*, or the identity element of the multiplicative group. As a reminder, the
identity element is an element that when multiplied against any other element leaves that element untouched. We will
call the remainders whose square is the identity to be of "order 2". Now notice that for $N = 8$, all the non identity
elements in the multiplicative group, which are the remainders 3, 5, and 7, are of order 2. This raises the question,
what other *N* is this true for? Or to be more precise, the question is this, what is the largest *N* such that the
every remainder mod *N* that has no common factor with *N* and is not equal to *1* has order 2?

Why is such a question of interest? In mathematics group elements of order 2 are associated with symmetries. For
example, if you flip a card and then flip it again, you have acted on it by group action of order 2. Symmetries in
mathematics and physics show up everywhere and tend to be fundamental in the theories. Specifically for physics,
symmetries, which are also dualities, are foundational. At every place you look you find examples, such as positive and
negative charges, up and down spin states for particles, clockwise vs. counterclockwise rotation, and on and on. The
derivation of special relativity uses symmetries as its primary driving assumption. They are also at the foundation of a
lot of the richer theories in mathematics as well. In our particular case, suppose we have determined the *N* that is
the answer to the question, would primes that map to the identity element mod *N* have any special properties? And what
does this have to do with the prime *73*? If you have been reading any of the prior blog articles, you may be able to
guess the answer. An answer can be found at [Largest N with All Order 2 Multiplicative Group Mod
N](https://gist.github.com/sampwhite/a22e248bd724a46dc3796ea8be428196).
