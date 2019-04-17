---
layout: post
mathjax: true
tags: math, python
---

# Pocket change combinations

Here's a puzzle involving pocket change: assuming you have coins of 2c and 5c in infinite amounts, how many different ways can you combine them to pay a 50c cup of coffee with the exact amount?

There's probably some kind of dynamic programming algorithm for solving this sort of stuff but what I wanted to show is more fun.

I'll just start by pulling this function out of nowhere:

$$ F(X) = \frac{1}{(1 - X^{2})(1 - X^{5})} $$

... but why? The explanation is on its way. For now I'd like to look at how $F$ behaves around $X = 0$; more specifically its _series expansion_ at 0. For values of X _close enough_ to 0 you get the following approximation using $F$'s first derivative:

$$ F(X) \approx F(0) + F'(0)X $$

You can refine the approximation by adding higher powers of $X$, with multipliers that involve $F$'s higher order derivatives:

$$ F(X) \approx F(0) + F'(0)X + \frac{F''(0)}{2}X^2 + \frac{F''(0)}{6}X^3$$

The full series expansion goes like:

$$ F(X) = F(0) + F'(0)X + \frac{F''(0)}{2}X^2 + \frac{F^{(3)}(0)}{6}X^3 + \frac{F^{(4)}(0)}{24}X^4 + ...+ \frac{F^{(n)}(0)}{n!}X^n + ...$$

**Now what's the point of all this?**

Turns out, the multiplier in front of $X^{50}$ in F's series expansion at 0 is [ _**drum roll**_ ] the number of possible combinations of 2c and 5c coins for paying that coffee!

Don't believe me? Here are the ways 2c and 5c coins add up to 50c:

- $10 \cdot 5$
- $8 \cdot 5 + 5 \cdot 2$
- $6 \cdot 5 + 10 \cdot 2$
- $6 \cdot 5 + 10 \cdot 2$
- $6 \cdot 5 + 10 \cdot 2$
- $6 \cdot 5 + 10 \cdot 2$

That's all **6** of them. And the $50^{th}$ term in F's series expansion?

Since I don't feel like calculating derivatives by hand I'll just delegate to Sympy:

~~~python
import sympy as sp

x = sp.Symbol("x")
f = 1 / ((1 - x ** 2) * (1 - x ** 5))
u = sp.series(f, n=51)

>>> 1 + x**2 + x**4 + x**5 + x**6 + x**7 + x**8 + x**9 + 2*x**10 + x**11 + 2*x**12 + x**13 + 2*x**14 + 2*x**15 + 2*x**16 + 2*x**17 + 2*x**18 + 2*x**19 + 3*x**20 + 2*x**21 + 3*x**22 + 2*x**23 + 3*x**24 + 3*x**25 + 3*x**26 + 3*x**27 + 3*x**28 + 3*x**29 + 4*x**30 + 3*x**31 + 4*x**32 + 3*x**33 + 4*x**34 + 4*x**35 + 4*x**36 + 4*x**37 + 4*x**38 + 4*x**39 + 5*x**40 + 4*x**41 + 5*x**42 + 4*x**43 + 5*x**44 + 5*x**45 + 5*x**46 + 5*x**47 + 5*x**48 + 5*x**49 + 6*x**50 + O(x**51)
~~~

~~~python
print(u.coeff(x ** 50))

>>> 6
~~~

Alright, maybe it wasn't _that_ convincing. Even though I don't feel like giving a formal proof for this - I probably wouldn't be able to write it down properly anyway - I should at least try to show how it works.

## Explanations


## Discussion

To be honest this probably isn't the most efficient method for figuring for solving this problem. 

The proper term for describing decompositions of positive integers into sums of smaller positive integers is a **partition**. You'll find a solid introduction to this in [REF]

As for this formula right here:

$$ \frac{1}{1 - X} = 1 + X + X^{2} + X^{3} + X^{4} ... $$

The way I introduced it is kind of questionable since the sum on the right-hand side does not converge for $|X| > 1$. The proper concept for manipulating these expressions without worrying about convergence issues is called **formal power series**. More on this topic in [REF].