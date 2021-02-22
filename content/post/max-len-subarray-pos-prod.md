---
author: Alex Sieusahai
title: Maximum Length of Subarray With Positive Product
date: 2020-09-01
description:
---

This is [this Leetcode problem](https://leetcode.com/problems/maximum-length-of-subarray-with-positive-product/).

## Statement

Given an array of integers `nums`, find the maximum length of a (contiguous) subarray where the product of all elements is positive.

## Solution

Due to the constraints of the problem, we know that an O(n) solution, at least, is expected.
Due to this, there must be some kind of structure which we can utilize in the problem for faster computation of the desired problem. 
So, let's imagine what information we have available to us doing a forward pass, for instance.
We know every previous number.
What useful information could that give us?
Well, imagine if we could answer the problem of the maximum length contiguous subarray where the product of all elements is positive _given that the last element is used_.
Then, we could very easily solve the problem and _anything we wanted to append onto it_; this is the key insight for this problem.

So, let's first convince ourselves that if we had all of the solutions to the above, we'd have the solution to the original problem.
This is quite easy; the solutions to the original problem for each subarray `nums[:i]` for all `i` necessarily contain the solution to the original problem; indeed, it should be the maximmum solution to this new problem.

There's another aspect that is important, in that if `nums[i]` is negative and `nums[:i-1]` has a subarray which has a negative product, then `nums[:i-1].append(nums[i])` has a subarray with a positive product.
So, we have to solve for both the positive and negative products at the same time.

For convenience, let's denote `max_pos(i-1)` and `max_neg(i-1)` to be the solutions to both of the problems for `nums[:i-1]`.

Okay, so now we can start thinking more about the implementation, now that a high level plan has been sketched.
Let's consider what happens when `nums[i]` is positive.
Then, we have that  
`max_pos(i) = 1 + max_pos(i-1)`  
`max_neg(i) = 1 + max_neg(i-1) if max_neg(i-1) > 0 else 0`

Why the `if max_neg(i-1) > 0`? In the case of no negative array found from before, there's no negative array to append on; ie, `nums[:i-1]` has no subarray with a negative product.

Let's consider what happens when `nums[i]` is negative.
Then, we have that  
`max_pos(i) = 1 + max_neg(i-1) if max_neg(i-1) > 0 else 0`  
`max_neg(i) = 1 + max_pos(i-1)`

Now that we have everything in place, the solution is just a matter of implementation:

```python3
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        max_pos, max_neg = {-1: 0}, {-1: 0}
        for i, x in enumerate(nums):
            if x == 0:
                max_pos[i] = max_neg[i] = 0
            elif x > 0:
                max_pos[i] = 1 + max_pos[i-1]
                max_neg[i] = 1 + max_neg[i-1] if max_neg[i-1] else 0
            else:  # x < 0
                max_pos[i] = 1 + max_neg[i-1] if max_neg[i-1] else 0
                max_neg[i] = 1 + max_pos[i-1]
        return max(max_pos.values())
```
