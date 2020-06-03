---
author: Alex Sieusahai
title: Markov's Inequality
date: 2020-06-03
description: What it is, and how to prove it.
---

Sometimes, it might be useful for us to bound the probability of some random variable $X$ being bigger than some number $\epsilon$. Thus, people have established some machinery to help us deal with this case, by expressing $\mathbb{P}(|X| \geq \epsilon)$ in terms of its expectation and $\epsilon$.

This is exercise 5.2 in Lattimore's _Bandit Algorithms_.

## Markov's Inequality
Let $X$ be a (discrete) random variable, and $\epsilon \geq 0$. We then have that 
$$ \mathbb{P}(|X| \geq \epsilon) \leq \frac{\mathbb{E}[|X|]}{\epsilon} $$

### Proof

Assume, just for the sake of simplicity, that $x \geq 0 \forall x \in X$.  
Note that relationship between $\mathbb{P}(X \geq \epsilon)$ and $\mathbb{E}[X]$ is actually quite simple:
$$ \mathbb{P}(X \geq \epsilon) = \sum\_{x \in X} p\_X(x) \mathbb{I}\\{x \geq \epsilon\\} $$
Now, we've filtered our $x$'s so that every single $x \geq \epsilon \implies \frac{x}{\epsilon} \geq 1$, which is very convenient. We can just extend what we have above:
$$ \sum\_{x \in X} p\_X(x) \mathbb{I}\\{x \geq \epsilon\\} \leq \sum\_{x \in X} p\_X(x) \mathbb{I}\\{x \geq \epsilon\\} \frac{x}{\epsilon} $$
Finally, since we have that $x \geq 0 \forall x \in X$, we have that
$$ \sum\_{x \in X} p\_X(x) \mathbb{I}\\{x \geq \epsilon\\} \frac{x}{\epsilon} \leq \sum\_{x \in X} p\_X(x) \frac{x}{\epsilon} = \frac{\mathbb{E}[X]}{\epsilon}. $$
This completes our proof, as we have the desired result!
