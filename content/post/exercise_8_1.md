---
author: Alex Sieusahai
title: Exercise 8.1 of Bandit Algorithms
date: 2020-08-19
description:
---

## Statement
Show that
$$ \sum\_{t=1}^n (1/f(t)) \sum\_{s=1}^n exp(-s\epsilon^2/2) \leq 5/\epsilon^2, $$
where $f(t) = 1 + t log^2(t)$.

### Solution
This follows the structure of the hints given.

First, begin by noticing that:

$$ \sum\_{s=1}^n exp(-s\epsilon^2/2) = exp(-\epsilon^2/2) + exp(-2\epsilon^2/2) + exp(-3\epsilon^2/2) + ... + exp(-n\epsilon^2/2) (\*) $$
And thus, this is a geometric series with the ratio of $r = exp(-\epsilon^2/2)$. Recall that
$$ r + r^2 + r^3 + r^4 + ... + r^n) = \frac{r}{1-r}, $$
so we can rewrite (\*) as
$$ \sum\_{s=1}^n exp(-s\epsilon^2/2) = \frac{exp(-\epsilon^2/2)}{1 - exp(-\epsilon^2/2)}. $$

It's now probably in our best interest to somehow bound $exp(-a) / (1 - exp(-a))$. In particular, we can expand it to obtain:
$$ \frac{e^{-a}}{1-e^{-a}} = e^{-a} * (\frac{e^{a} - 1}{e^{a}})^{-1} = (e^a - 1)^{-1} $$
We want to show that
$$ \frac{e^{-a}}{1-e^{-a}} = (e^a - 1)^{-1} \leq 1/a \implies a \leq e^a - 1 \implies 1 + a \leq e^a. $$
We can do this very easily using the Taylor Series of $e^x = \sum\_{i=0}^\infty \frac{x^i}{i!}$:

$$ e^a = 1 + a + \sum\_{i=2}^\infty \frac{a^i}{i!} \geq 1 + a \forall a \geq 0 $$

So, we have that $\frac{e^{-a}}{1-e^{-a}} \leq 1/a$.

To recap, we currently have that

$$\sum\_{s=1}^n exp(-s\epsilon^2/2) = \frac{exp(-\epsilon^2/2)}{1 - exp(-\epsilon^2/2)} \leq \frac{2}{\epsilon^2}. $$

So, we just need to figure out a reasonable way to bound $\sum\_{t=1}^n f(t)^{-1}$.

An important fact to note is that $f(t)^{-1} \leq tlog^2t$ (given to us in the hints) and thus we can write the following, for some $k$ which we have yet to determine:

$$ \sum\_{t=1}^n (f(t))^{-1} \leq \sum\_{t=1}^k (f(t))^{-1} + \sum\_{t=k+1}^n (tlog^2t)^{-1} \leq \sum\_{t=1}^k (f(t))^{-1} + \int\_{k+1}^n (tlog^2t)^{-1} dt $$

We can solve the integral fairly easily with a u-substitution of $u = logt \implies du = dt/t$:
$$ \int\_{k+1}^n (tlog^2t)^{-1} dt = \int\_{log(k+1)}^{log(n)} u^{-2} du = -log(x)|\_{x=log(k+1)}^{log(n)} = log(k+1) - log(n) = log(\frac{k+1}{n}) $$

In order to obtain the bounds desired, we must select $k$ such that
$$ \sum\_{t=1}^n (f(t))^{-1} = \sum\_{t=1}^k (tlog^2t)^{-1} + \int\_{k+1}^n (tlog^2t)^{-1} dt = \sum\_{t=1}^k (f(t))^{-1} + log(k+1) - log(n) \leq \frac{5}{2} $$

One could be found with just brute force computation. Finally, we have our result:

$$ \sum\_{t=1}^n (f(t))^{-1} \sum\_{s=1}^{n} exp(-s\epsilon^2/2) \leq \sum\_{t=1}^n (f(t))^{-1} 2/\epsilon^2 \leq 5/2 * 2/\epsilon^2 = 5/\epsilon^2 $$

