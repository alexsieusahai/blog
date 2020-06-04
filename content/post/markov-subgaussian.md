---
author: Alex Sieusahai
title: A Markov's Inequality-like Bound For Subgaussian variables
date: 2020-06-04
description:
---

It's fairly easy to show that for any $\sigma$-subgaussian random variable $X$ and $\epsilon \geq 0$, we have that
$$ \mathbb{P}(X \geq \epsilon) \leq e^{- \frac{\epsilon^2}{2 \sigma^2}} $$
To prove it, just notice that $f(x) = e^{\lambda x}$ is monotonically increasing, and thus we can condlude that for arbitrary $\lambda > 0$:
$$ \mathbb{P}(X \geq \epsilon) = \mathbb{P}(e^{\lambda X} \geq e^{\lambda \epsilon}) $$
Notice that we have $e^{\lambda X}$ on the left of our inequality, and recall that for any $\sigma$-subgaussian random variable, we have that
$$ \mathbb{E}[e^{\lambda X}] \leq e^{\lambda^2 \sigma^2 / 2} $$
We'd like to incorporate that expectation, and we can do so using Markov's Inequality:
$$ \mathbb{P}(e^{\lambda X} \geq e^{\lambda \epsilon}) \leq \mathbb{E}[e^{\lambda X}] e^{-\lambda \epsilon} $$
Let's apply the definition of subgaussianity and we're getting pretty close:
$$ \mathbb{E}[e^{\lambda X}] e^{-\lambda \epsilon} \leq e^{\lambda^2 \sigma^2 / 2 - \lambda \epsilon} $$
Now, we're almost there, we just have to be a little clever. Recall that we've assumed arbitrary $\lambda > 0$. In particular, the statement of the result does not require $\lambda$ to take on an arbitrary value, so we can select our value of $\lambda$ to help us obtain the desired result.
In particular, select $\lambda = \epsilon / \sigma^2$.
We then obtain the desired bound, which completes the proof!
