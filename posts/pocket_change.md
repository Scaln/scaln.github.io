---
layout: post
mathjax: true
---

# Pocket change combinations

Here's a puzzle involving pocket change: assuming you have coins of 2c and 5c in infinite amounts, how many different ways can you combine them to pay a 50c cup of coffee with the exact amount?

There's probably some kind of dynamic programming algorithm for solving this sort of stuff but what I wanted to show is more fun. It also involves a bit of extra math.

I'll just start by pulling this function out of nowhere:

$$ F(X) = \frac{1}{(1 - X^{2})(1 - X^{5})} $$

... but why? The explanation is on its way. For now I'd like to look at how $F$ behaves around $X = 0$; more specifically its _series expansion_ at 0. For values of X _close enough_ to 0 you get the following approximation using $F$'s first derivative:

$$ F(X) = F(0) + F'(0)X $$

This approximation can be refined by adding higher powers of $X$, with multipliers that involve $F$'s higher-order derivatives.

$$ F $$

Now what's the point of all this?

It turns out $a_{50}$, the multiplier in front of $X^{50}$ in F's series expansion at 0 is [_**drum roll**_] the number of possible combinations of 2c and 5c coins for paying that coffee!

Don't believe me? Here are the ways 2c and 5c coins add up to 50c:

- $10 \cdot 5$
- $8 \cdot 5 + 5 \cdot 2$
- $6 \cdot 5 + 10 \cdot 2$
- $6 \cdot 5 + 10 \cdot 2$
- $6 \cdot 5 + 10 \cdot 2$
- $6 \cdot 5 + 10 \cdot 2$

So 6 total. And the $50^{th}$ term in F's series expansion?

Since I don't feel like calculating derivatives by hand I'll just delegate to Sympy:

~~~python
import sympy as sp

def expand():
~~~

Alright, maybe it wasn't _that_ convincing. Even though I don't feel like giving a formal proof for this - I probably wouldn't be able to write it down properly anyway - I should at least try to show how it works.

## Explanations


## More on the theory

I deliberately left a few parts "unclean" in order to keep things simple.

The proper term for describing decompositions of positive integers into sums of smaller positive integers is a **partition**. You'll find a solid introduction to this in [REF]

As for this formula right here:

$$ \frac{1}{1 - X} = 1 + X + X^{2} + X^{3} + X^{4} ... $$

The way I introduced it is kind of questionable since the sum on the right-hand side does not converge for $|X| > 1$. The proper concept for manipulating these expressions without worrying about convergence issues is called **formal power series**. More on this topic in [REF].