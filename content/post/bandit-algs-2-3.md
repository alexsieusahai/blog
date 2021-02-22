---
author: Alex Sieusahai
title: Exercise 2.3 of Bandit Algorithms
date: 2020-11-21
description:
---

## Statement
Given an arbitrary set $\mathscr{U}$, a measurable space $(\mathscr{V}, \Sigma)$ and an arbitrary function $X : \mathscr{U} \to \mathscr{V}$, 
show that $\Sigma_X = \{ X^{-1}(A) : A \in \Sigma \} $ is a $\sigma$-algebra over $\mathscr{U}$.

### Solution

This amounts to showing the following, directly from the definition of what it means for some set $\Sigma_X$ to be a $\sigma$-algebra of $\mathscr{U}$:

* $\mathscr{U} \in \Sigma_X$
* Closure under complementation
* Closure under countable unions

First, we will show that $\mathscr{U} \in \Sigma_X$.
Note that $\Sigma$ is a $\sigma$-algebra, and thus $\mathscr{V} \in \Sigma$.
If this is true, then we must have that $X^{-1}(\mathscr{V}) = \mathscr{U} \in \Sigma\_X$ by definition of $\Sigma\_X$.

Next, we will show closure under complementation.
To be more precise, we will show that for any $B \in \Sigma_X$, we have that $B^C = \mathscr{U} / B \in \Sigma\_X$.
Let $B \in \Sigma\_X$ be arbitrary. 
We then have that there exists some $A \in \Sigma$ such that $X^{-1}(B) = A$ by the definition of $\Sigma\_X$.
Since $\Sigma$ is a $\sigma$-algebra, we have that $A^C \in \Sigma$.
So we have that some $B' = X(A^C) \in \Sigma\_X$.
We will show that $B' = B^C$.

First, we will show that $B \cup B' = \mathscr{U}$. 
Let $x \in \mathscr{U}$ be arbitrary.
Suppose $x \not \in B$. 
Then we have that $X^{-1}(x) \not \in A$, and $X^{-1}(x) \in \mathscr{V}$.
Thus, we must have that $X^{-1}(x) \in A^C$, by the definition of complement.
Since $X^{-1}(x) \in A^C$, we have that $x \in X(A^C) = B'$. 
Very similar logic holds for the other case.
Then we have that $B \cup B' = \mathscr{U}$.

We will now show that $B \cap B' = \emptyset$.
We will show this by contradiction.
Suppose $x$ is such that $x \in B$ and $x \in B'$.
Then we have that $X^{-1}(x) \in A$ and $X^{-1}(x) \in A^C$, which is a contradiction.

Thus, we have that $B' = B^C$, where $B' \in \Sigma\_X$; we have closure under complementation.

Lastly, we will show closure under countable union.
To be more precise, we will show that for any $A\_i$ such that $A\_i \in \Sigma\_X$ for all $i \in \mathbb{N}$, $ \cup\_{i \in \mathbb{N}} \in \Sigma\_X$.
Let $A\_i$ be such that $A\_i \in \Sigma\_X$ for all $i \in \mathbb{N}$.
Then, we have that for each $A_i$, there exists a $B\_i = X^{-1}(A\_i) \in \Sigma$.
Since $\Sigma$ is a $\sigma$-algebra, we have that $\cup\_{i \in \mathbb{N}} B\_i \in \Sigma$.
By the definition of $\Sigma\_X$, we have that $ X^{-1}( \cup\_{i \in \mathbb{N}} B\_i ) = \cup\_{i \in \mathbb{N}} X^{-1} (B\_i) = \cup\_{i \in \mathbb{N}} A\_i \in \Sigma\_X$, as required.
So, we have closure under countable union.

Since we have $\mathscr{U} \in \Sigma_X$, closure under complementation and closure under countable union, we have that $\Sigma\_X$ is indeed a $\sigma$-algebra over $\mathscr{U}$.
