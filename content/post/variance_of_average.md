---
author: Alex Sieusahai
title: Variance Of Average
date: 2020-06-03
description: What it is, and how to prove it.
---

This is exercise 5.1 from Lattimore's _Bandit Algorithms_.

## Statement

Let $X_1, X_2, ..., X_n$ be a sequence of iid. random variables with mean $\mu$ and variance $\sigma^2 < \infty$. 
Let $ \hat{\mu} = \frac{1}{n} \sum\_{t=1}^n X_t $ and show that $ \mathbb{E}[(\hat{\mu} - \mu)^2] = \sigma^2/n $.

## Proof

This falls quite easily utilizing two important properties of variance:
$$ \mathbb{V}[aX] = a^2 \mathbb{V}[X] \text{ for arbitrary $a \in \mathbb{R}$ }  (\*) $$
$$ \mathbb{V}[\sum\_{t=1}^n X_t] = \sum\_{t=1}^n \mathbb{V}[X_t] (\*\*) $$

So, we are calculating $\mathbb{V}[ \frac{1}{n} \sum\_{t=1}^n X_t ]$.
First, use (\*) to obtain
$$ \mathbb{V}[ \frac{1}{n} \sum\_{t=1}^n X_t ] = \frac{1}{n^2} \mathbb{V} [\sum\_{t=1}^n X_t]. $$
Then, use (\*\*) to obtain
$$ \frac{1}{n^2} \mathbb{V} [\sum\_{t=1}^n X_t] = \frac{1}{n^2} \sum\_{t=1}^n \mathbb{V}[X_t] = \frac{1}{n^2} n \sigma^2 = \frac{\sigma^2}{n}, $$
as required. This completes the proof!
