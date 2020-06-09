---
author: Alex Sieusahai
title: A First Glance At Explore Then Commit
date: 2020-06-09
description: What it is, and a simple, important bound on the regret.
---

The Explore Then Commit bandit algorithm is conceptually very simple and easy to analyze; we first _explore_ all $k$ arms $m$ times, then proceed to exploit the arm with the highest sample mean after that.

For simplicity, let's just assume that each arm is 1-subgaussian. 
We can deal with the case of $\sigma$-subgaussian arms pretty easily by noting that for a $\sigma$-subgaussian random variable $X$, we have that $cX$ is $|c|\sigma$-subgaussian for arbitrary $c$. 
It's also probably easy to deal with the case where each arm has a different $\sigma$, as it probably just ends up being bookkeeping in whatever you're trying to prove. 
In the case where the subgaussian assumption is not met, alternative inequalities tailored for your specific distribution will have to be employed to get similar bounds.
 In contrast, if we know more about our random variables other than the loose assumption that the random variable is subgaussian, we might be able to obtain tighter bounds than what is proven here.
A trivial example of this would be arms that have a fixed payout; each arm would only have to be played once before we know for sure which arm is the best, if we know that each arm has a fixed payout prior to playing the bandit.

This algorithm seems very simple, but I don't think it's obvious as to whether or not it's effective.
For example, if $m$ is very high, then we almost certainly have found the best arm, but we've spent a lot of time exploring. 
Likewise, if $m$ is low, we probably won't find the best arm, but we've spent a small amount of time exploring. 
Due to this exploration / exploitation tradeoff and the simplicity of the problem, there might be a reasonable way to select the minimum $m$ so that the optimal arm (or possibly near optimal) is found after exploring with a given probability, for instance.
Finding a bound for the regret might be a reasonable first step to analyzing this algorithm, as then we can at least compare this algorithm to more sophisticated choices.

## Statement
When Explore Then Commit is interacting with any 1-subgaussian bandit and $1 \leq m \leq n/k$, we have that
$$ R_n \leq m \sum\_{i=1}^k \Delta_i + (n - mk) \sum\_{i=1}^k \Delta_i exp( - \frac{m \Delta_i^2}{4} ). $$

### Proof

For simplicity, lets assume (without loss of generality) that the first arm is the optimal arm.

The first thing to note is that we've seperated the regret up into calculating it for each action then summing over, so the Regret Decomposition Lemma is probably really useful here. 
With respect to that, we have

$$ R_n = \sum\_{i=1}^k \Delta_i \mathbb{E}[T_i(n)]. $$

From the Explore Then Commit algorithm definition, we can easily find out $\mathbb{E}[T_i(n)]$ for each arm. 
Recall that we play each arm $m$ times, then we keep playing it only if it has the highest sample mean:

$$ \mathbb{E}[T_i(n)] = m + (n - mk) \mathbb{P}(\hat{\mu}\_i \geq \hat{\mu_1} ) $$

Now, we're getting closer, the main piece of the puzzle which we haven't really figured out yet is how we're going to get from that probability to a bound involving $\Delta_i$.
The only machinery we have to get from probability to hard values is Markov's Inequality (and results derived from Markov's Inequality). 
In particular, there's a result proven [earlier](https://alexsieusahai.github.io/post/markov-subgaussian/) that seems like it would be very useful.
It also seems hard to deal with the two sample means directly, and a known property that's easy to prove about $\sigma$ subgaussian variables is that if we take two of them and take the sum of difference of them, the resulting variable is $\sqrt{2}\sigma$-subgaussian.
Thus, we can do the following to the probability:

$$ \mathbb{P}(\hat{\mu}\_i \geq \hat{\mu_1}) = \mathbb{P}(\hat{\mu}\_i - \hat{\mu_1} + \Delta_i \geq \Delta_i) $$

Now, we know the definition of the sample mean in this context is:

$$ \hat{\mu_i} = \sum\_{j=1}^m X\_{i, j} $$

Note that $\hat{\mu}\_i - \hat{\mu_1} + \Delta_i = \sum\_{j=1}^m [ X\_{i, j} - X\_{1, j} + \mu_1 - \mu_i ]$ which is then $\sqrt{2m}/m = \sqrt{2/m}$-subgaussian. Finally, using the aforementioned result proven earlier, we have that 
$$ \mathbb{P}(\hat{\mu}\_i - \hat{\mu_1} + \Delta_i \geq \Delta_i) \leq exp(-\Delta_i^2 m / 4).  $$

Now, to recap, we have that:

$$ \mathbb{E}[T_i(n)] = m + (n - mk) \mathbb{P}(\hat{\mu}\_i \geq \hat{\mu_1} ) \leq m + (n - mk)exp(-\Delta_i^2 m / 4) $$

And thus we have that

$$ R_n = \sum\_{i=1}^k \Delta_i \mathbb{E}[T_i(n)] \leq \sum\_{i=1}^k \Delta_i [m + (n - mk)exp(-\Delta_i^2 m / 4)], $$

as required!
