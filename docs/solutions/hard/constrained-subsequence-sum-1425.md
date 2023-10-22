---
title: Constrained Subsequence Sum
description: A solution to LeetCode Probelm 1425
icon: material/check
comments: true
tags:
    - Hard
    - Array
    - Dynamic Programming
    - Queue
    - Sliding Window
    - Heap (Priority Queue)
    - Monotonic Queue
---


# Constrained Subsequence Sum

## Description
!!! question "LeetCode Problem 1425: Constrained Subsequence Sum"
    Given an integer array `nums` and an integer `k`, you need to find the maximum sum of a non-empty subsequence of the array, such that for any two consecutive integers in the subsequence, `nums[i]` and `nums[j]` (where `i < j`), the condition `j - i <= k` is satisfied. In simpler terms, the elements you choose in the subsequence should be within a maximum distance of `k` from each other.

    **Example:**
    ```python
    Input: nums = [10, 4, 8, 1, 2, 6], k = 2
    Output: 18
    Explanation: The maximum sum subsequence is [10, 4, 4], where the distance between any two elements is at most 2.


## Intuition
We aim to find the maximum sum of a non-empty subsequence with the constraint that the difference between two consecutive elements in the subsequence should be less than or equal to 'k'. To achieve this, we'll use dynamic programming and a double-ended queue (deque) to optimize the solution.

## Approach
1. Initialize a deque to store the indices of maximum values within the last 'k' elements.
2. Create an array 'dp' to store the maximum sum for subsequences ending at each index.
3. Iterate through the 'nums' array.
   - Check and update the deque to maintain valid indices within the last 'k' elements.
   - Calculate the maximum sum for the current index 'i' by considering the value at 'i' and the maximum value from the deque.
   - Update the 'dp' array and the deque as we progress through the array.
4. The result is the maximum value in the 'dp' array.

## Complexity
- Time complexity: O(n) - We iterate through the 'nums' array once.
- Space complexity: O(n) - We use an array 'dp' and a deque for storage.

```swift
class Solution {
    func constrainedSubsetSum(_ nums: [Int], _ k: Int) -> Int {
        let n = nums.count
        var dp = Array(repeating: 0, count: n)
        dp[0] = nums[0]
        
        // Deque to store the indices of maximum values in the last 'k' elements
        var deque = [0]
        
        for i in 1..<n {
            // Check if the maximum value in the deque is out of range
            while !deque.isEmpty && deque.first! < i - k {
                deque.removeFirst()
            }
            
            dp[i] = max(dp[deque.first!], 0) + nums[i]
            
            // Remove values from the back of the deque that are less than the current dp[i]
            while !deque.isEmpty && dp[i] >= dp[deque.last!] {
                deque.removeLast()
            }
            
            deque.append(i)
        }
        
        return dp.max()!
    }
}
