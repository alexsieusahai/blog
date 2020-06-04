---
author: Alex Sieusahai
title: The Expectation of Subgaussian Random Variables
date: 2020-06-03
description: What it is, and how to prove it.
---

This is a portion of exercise 5.7 in Lattimore's _Bandit Algorithms_. The rest of the results are not too hard to prove, but I found this portion of exercise 5.7 the most challenging by far.

A random variable $X$ is considered to be _$\sigma$-subgaussian_ if for all $\lambda$,
$$ \mathbb{E}[e^{\lambda X}] \leq e^{\lambda^2 \sigma^2 / 2}. $$

A known property of subgaussian random variables is that their expectation is 0. How could that be? 
Well, let's think about how we could possibly obtain $\mathbb{E}[X]$ in some way from the definition.

Well, something we could use to obtain $\mathbb{E}[X]$ would be to utilize the power series representation of $e^x$:
$$ e^x = \sum\_{n=0}^\infty \frac{x^n}{n!} $$
As a result, we have that
$$ \mathbb{E}[e^{\lambda X}] = \mathbb{E}[ \sum\_{n=0}^\infty \frac{(\lambda X)^n}{n!} ] $$
Using the linearity of expectation, we have that
$$ = 1 + \mathbb{E}[X] + \lambda^2 \mathbb{E}[X^2] / 2 + ... $$
Using the definition of subgaussianity, we have that
$$ \leq e^{\lambda^2 \sigma^2 / 2} = 1 + (\lambda\sigma)^2 / 2 + (\lambda\sigma)^4 / 4 + ... $$
So, to recap, we now have
$$ 1 + \lambda \mathbb{E}[X] + \lambda^2 \mathbb{E}[X^2] / 2 + ... \leq e^{\lambda^2 \sigma^2 / 2} = 1 + (\lambda\sigma)^2 / 2 + (\lambda\sigma)^4 / 4 + ... $$
$$ \lambda \mathbb{E}[X] + \lambda^2 \mathbb{E}[X^2] / 2 + ... (\lambda\sigma)^2 / 2 + (\lambda\sigma)^4 / 4 + ... (\*) $$
So, if we divide everything by $\lambda > 0$, we have
$$ \mathbb{E}[X] + \lambda \mathbb{E}[X^2] / 2 + ... \leq \lambda\sigma^2 / 2 + \lambda^3\sigma^4 / 4 + ... $$
Thus, if we take $\lambda \to 0^+$, we have that $\mathbb{E}[X] \leq 0$.

If we revisit (\*), and instead divide everything by $\lambda < 0$, we have
$$ \mathbb{E}[X] + \lambda \mathbb{E}[X^2] / 2 + ... \geq \lambda\sigma^2 / 2 + \lambda^3\sigma^4 / 4 + ... $$
Thus, if we take $\lambda \to 0^-$, we have that $\mathbb{E}[X] \geq 0$.

So, for arbitrary $\lambda$, we have that $0 \leq \mathbb{E}[X] \leq 0 \implies \mathbb{E}[X] = 0$. This completes the proof!
