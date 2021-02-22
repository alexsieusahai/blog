---
author: Alex Sieusahai
title: Find Latest Group Of Size M
date: 2020-09-03
description:
---

This is [this Leetcode problem](https://leetcode.com/problems/find-latest-group-of-size-m/).

## Statement

Given an array `arr` that represents a permutation of numbers from `1` to `n`. 
You have a binary string of size `n` that initially has all its bits set to zero.

At each step `i` (assuming both the binary string and `arr` are 1-index) from `1` to `n`, the bit at position `arr[i]` is set to `1`.
You are given an integer `m` and you need to find the latest step at which there exists a group of ones of length `m`.
A group of ones is contiguous substring of 1s such that it cannot be extended in either direction.

Return _the latest step at which there exists a group of ones of length **exactly**_ `m`. If no such group exists, return `-1`.

## Solution

Something that screams out initially is that we are filling out a graph over time by introducing nodes, followed by grouping adjacent nodes together.
The simplest way to keep track of all of this joining information in an efficient way is the union-find data structure.
Intuitively, when we flip a bit from 0 to 1, we drop that node on the graph, and it contains only itself.
Then, we look to the left, and if there's a group, we join it. 
Then, we look to the right, and if there's a group, we join it.

We need some way to conveniently join things together.
A simple way to do this is to have all nodes in a group point to some other node in a way so that if we keep following this pattern, we'll reach a node that points to itself.
It's as if we're following a bunch of paths upward toward an identifier tag.

We can keep track of what points to what via a `parent` mapping, which maps a node to the next node in the path described above.
When we join two groups (first and second) together, we have the `root_first` (the node at the end of each path described above for the first group) of the first group satisfy `parent[root_first] = root_second`.
Then, when we generate the path for anything that was in first, we'll come across `root_first`, which will then point to `root_second`.

In this problem, we also have to keep track of the size of groups.
Since we join groups together, we can also add their counts together using similar logic as above.

We want to find the largest `i` such that there exists one group of nodes that is of size `m`.
Thus, keeping track of the size of each group is sensible (indeed, you could probably get away with just keeping track of groups of size `m`).
All we have to do is some more bookkeeping in the joining logic and we're good to go!

With all of the machinery laid out, here's the solution I found:

```python3
from collections import defaultdict

class Solution:
    def findLatestStep(self, arr: List[int], m: int) -> int:
        parent = {}
        sz = defaultdict(int)  # mapping from count to number of groups with that count
        count = {}  # mapping from parent to count
        def get_root(x):
            path = []
            while x != parent[x]:
                path.append(x)
                x = parent[x]
            for y in path:
                parent[y] = x
            return x
            
        def join(x, y):
            x_root, y_root = get_root(x), get_root(y)
            sz[count[y_root]] -= 1
            sz[count[x_root]] -= 1
            parent[x_root] = y_root
            count[y_root] += count[x_root]
            sz[count[y_root]] += 1
            
        best_soln = -1
        for i, x in enumerate(arr):
            parent[x], count[x] = x, 1
            sz[1] += 1
            
            if x-1 in parent:
                join(x, x-1)
            if x+1 in parent:
                join(x, x+1)
                
            if sz[m] > 0:
                best_soln = i + 1
            
        return best_soln
```
