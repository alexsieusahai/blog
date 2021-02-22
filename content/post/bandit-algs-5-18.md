---
author: Alex Sieusahai
title: Exercise 5.18 of Bandit Algorithms
date: 2020-12-04
description:
---

## Statement
Let $X_1, ..., X_n$ be a sequence of $\sigma$-subgaussian random variables and $Z = max\_{t \in [n]} X_t$. Prove that $\mathbb{E}[Z] \leq \sqrt{2 \sigma^2 log(n)}$.

### Solution

A key naive bound on the maximum of any set of real numbers is obviously the sum, which works nicely with the $log(n)$ term.

$ \mathbb{E}[Z] \leq \sum\_{t=1}^n \mathbb{E}[X_t] $
Let $\lambda > 0$. Since $ x \mapsto e^{\lambda x} $ is monotonic, we have that
$ e^{\lambda \mathbb{E}[Z]} \leq \sum\_{t=1}^n \mathbb{E}[X_t]. $
Using the definition of subgaussianity, we have that
$ e^{\lambda \mathbb{E}[Z]} \leq \sum\_{t=1}^n \mathbb{E}[X_t] \leq n e^{\lambda^2 \sigma^2 / 2}. $

So, taking the logarithm of both sides in the above inequality (which is permitted due to the monotonicity of logarithms), we have that

$ \lambda \mathbb{E}[Z] \leq log(n) + \lambda^2 \sigma^2 / 2 $
$ \mathbb{E}[Z] \leq log(n)/\lambda  + \lambda \sigma^2 / 2 $

Now, we just need a slightly clever choice for $\lambda$. We know that we want to find a choice for $\lambda$ so that
$ log(n)/\lambda  + \lambda \sigma^2 / 2 = \sqrt{2 \sigma^2 log(n)} $

So, we need the first term to have $\sqrt{log(n)}$ and $\sigma$ introduced; from this alone it's relatively simple to deduce that $\lambda = sqrt{2 log(n)} \sigma^{-1}$ suffices.
