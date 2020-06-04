---
author: Alex Sieusahai
title: Obtaining Tail Bounds For (Subgaussian) Random Variables
date: 2020-06-04
description:
---

This is exercise 5.5 of Lattimore's _Bandit Algorithms_.

## Statement
Let $X_1, X_2, ..., X_n$ be a sequence of iid. random variables with mean $\mu$, variance $\sigma^2$ and bounded third absolute moment, $\rho = \mathbb{E}[|X_1 - \mu|^3] < \infty$.
Let $S_n = \sum\_{t=1}^n (X_t - \mu) / \sigma$. The Berry-Esseen theorem shows that
$$ sup_x | \mathbb{P}(S_n / \sqrt{n} \leq x) - \Phi(x) | \leq C \rho / \sqrt{n}, $$
where $C < 1/2$ is a universal constant, and $\Phi$ is the CDF of a (0, 1)-Gaussian random variable.

Let $\hat{\mu}\_n = \frac{1}{n} \sum\_{t=1}^n X_t$. Find a bound of the form $\mathbb{P}(\hat{\mu}\_n \geq \mu + \epsilon)$ for positive values of $\epsilon$. 

### Solution
Note: I believe this is right but it's possible that I could have made errors. If you do notice them, please let me know via email or by making a new issue on the Github repository corresponding to this blog.  
This may seem a little intimidating but indeed it's actually quite simple. Recall that expectations are linear, so we have that for any $t \in \\{0, 1, ..., n\\}$, we have that
$$ \mathbb{E}[\frac{X_t}{n}] = \frac{\mu}{n}, \mathbb{V}[\frac{X_t}{n}] = \frac{\sigma^2}{n^2}. $$
Note also that $\sum\_{t=1}^n \mu/n = \mu$. So, we have that
$$ \hat{\mu} \geq \mu + \epsilon \implies \hat{\mu} - \mu \geq \epsilon \implies \sum\_{t=1}^n (\frac{X_t}{n} - \frac{\mu}{n}) \geq \epsilon \implies \frac{\sum\_{t=1}^n (\frac{X_t}{n} - \frac{\mu}{n})}{n^2 \sigma^2} = S_n \geq \frac{\epsilon}{n^2\sigma^2} \implies S_n / \sqrt{n} \geq \frac{\epsilon}{n^{5/2} \sigma^2} $$
For ease of notation, let $\alpha = \frac{\epsilon}{n^{5/2} \sigma^2}$. So, via Berry-Esseen, we have that
$$ | \mathbb{P}(S_n / \sqrt{n} \leq \alpha) - \Phi(\alpha) | \leq sup_x | \mathbb{P}(S_n / \sqrt{n} \leq x) - \Phi(x) | \leq C \rho / \sqrt{n} $$
and thus we have that (assuming $C > 0$)
$$ -C \rho / \sqrt{n} \leq \mathbb{P}(S_n / \sqrt{n} \leq \alpha) - \Phi(\alpha)  \leq C \rho / \sqrt{n} $$
Finally giving us our tail bounds
$$ -C \rho / \sqrt{n} + \Phi(\alpha) \leq \mathbb{P}(S_n / \sqrt{n} \leq \alpha) \leq C \rho / \sqrt{n} + \Phi(\alpha) $$
More succinctly stated, we have that
$$ \mathbb{P}(\hat{\mu} - \mu \geq \epsilon) = \mathbb{P}(S_n / \sqrt{n} \geq \frac{\epsilon}{n^{5/2}\sigma^2}) \leq C\rho / \sqrt{n} + \Phi(\frac{\epsilon}{n^{5/2}\sigma^2}). $$
