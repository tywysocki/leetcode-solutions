---
title: Wildcard Matching
description: A solution to LeetCode problem 45
icon: material/check
comments: true
tags:
    - Hard
    - Swift
    - String
    - Dynamic Programming
    - Greedy
    - Recursion
---

## Description
!!! question "LeetCode Problem 45. Wildcard Matching"
    Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*' where:

    - '?' Matches any single character.
    - '*' Matches any sequence of characters (including the empty sequence).

    The matching should cover the entire input string (not partial).

## Solution
We are given an input string `s` and a pattern string `p`, where the pattern can contain wildcard characters '?' and '*'. The problem is to determine if the pattern matches the entire input string. To solve this problem, we can use a recursive approach with memoization to avoid redundant calculations.

## Approach
We'll use a recursive function `isMatchRecursive` that checks if the pattern matches the input string. We have two pointers, one for the input string (`sIndex`) and one for the pattern (`pIndex`). We consider the following cases:

1. If `pIndex` reaches the end of the pattern, we check if `sIndex` also reached the end of the input string. If so, we have a match.

2. If `p[pIndex]` is '*', we have two choices:
    1. Use '*' to match nothing in the input string and increment `pIndex`.
    2. Use '*' to match one or more characters in the input string and increment `sIndex`.

3. If `p[pIndex]` is a regular character or '?', we check if it matches the current character in the input string (`s[sIndex]`). If there is a match, we increment both `sIndex` and `pIndex`.

We also use memoization to store the results of subproblems, preventing redundant calculations. If we have already solved a particular subproblem, we return the result from the memoization array.

## Complexity

### Time complexity:
The time complexity of this solution is $O(s * p)$, where s is the length of the input string and p is the length of the pattern. This is because in the worst case, we may need to fill in the entire memoization array, and each cell requires constant time to compute.

### Space complexity: 
The space complexity is $O(s * p)$ as well, since we use a memoization array to store the results of subproblems.

## Code

```swift
func isMatch(_ s: String, _ p: String) -> Bool {
    var memo = Array(repeating: Array(repeating: false, count: p.count + 1), count: s.count + 1)
    return isMatchRecursive(Array(s), Array(p), 0, 0, &memo)
}

func isMatchRecursive(_ s: [Character], _ p: [Character], _ sIndex: Int, _ pIndex: Int, _ memo: inout [[Bool]]) -> Bool {
    if pIndex == p.count {
        return sIndex == s.count
    }
    
    if memo[sIndex][pIndex] {
        return false
    }

    var match = false

    if p[pIndex] == "*" {
        match = isMatchRecursive(s, p, sIndex, pIndex + 1, &memo) || (sIndex < s.count && isMatchRecursive(s, p, sIndex + 1, pIndex, &memo))
    } else if sIndex < s.count and (s[sIndex] == p[pIndex] or p[pIndex] == "?") {
        match = isMatchRecursive(s, p, sIndex + 1, pIndex + 1, &memo)
    }

    memo[sIndex][pIndex] = !match

    return match
}
```