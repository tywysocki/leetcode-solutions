---
comments: true
tags:
    - Easy
    - Swift
    - Hash Table
    - Array
---

# Two Sum

!!! question "LeetCode Problem 1: Two Sum"

    The Two Sum problem is a classic algorithmic problem. Given an array of integers `nums` and an integer `target`, your task is to find two numbers in the array that add up to the `target`. You can assume that each input will have exactly one solution, and you may not use the same element twice. The answer can be returned in any order.

    **Examples:**

    1. Input: `nums = [2, 7, 11, 15], target = 9`
    Output: `[0, 1]`
    Explanation: Because `nums[0] + nums[1] == 9`, we return `[0, 1]`.

    2. Input: `nums = [3, 2, 4], target = 6`
    Output: `[1, 2]`

    3. Input: `nums = [3, 3], target = 6`
    Output: `[0, 1]`

## Intuition
To efficiently solve the Two Sum problem, we'll utilize a hash map to keep track of the numbers we've encountered and their indices while iterating through the input array. This approach allows us to swiftly determine if the complement of the current number (i.e., `target - currentNumber`) has been seen before.

## Approach
Here's a step-by-step explanation of the approach:

1. Create an empty dictionary to store numbers and their indices. This dictionary will help us check whether we've encountered a number and its index before.

2. Iterate through the input array `nums` using a for loop. As we traverse the array, we keep track of the current number and its index.

3. For each number `num` at index `i`, calculate the complement as `target - num`. The complement is the number we need to find in the array to reach the target.

4. Check if the complement is already present in the dictionary. If it is, that means we've seen it before. In this case, we've found our solution, and we can return the indices of the complement and the current number as `[dict[complement]!, i]`.

5. If the complement is not in the dictionary, add the current number and its index to the dictionary. This allows us to look up the index of the current number later if we encounter the complement.

6. If no valid solution is found after iterating through the entire array, return an empty array to indicate that there's no valid pair of numbers that sum up to the target.

## Complexity
Let's analyze the time and space complexity of our solution:

### Time Complexity
The time complexity of this solution is O(n), where `n` is the number of elements in the input array `nums`. We iterate through the array once, checking and updating the dictionary.

### Space Complexity
The space complexity is O(n) as well. In the worst case, we may need to store all the elements and their indices in the dictionary, resulting in linear space usage.

## Code
Here's the Swift code implementing the Two Sum problem solution using the described approach:

```swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        // Create a dictionary to store numbers and their indices
        var numIndices = [Int: Int]()
        
        // Iterate through the array
        for i in 0..<nums.count {
            let num = nums[i]
            let complement = target - num
            
            // If the complement is in the dictionary, return the indices
            if let complementIndex = numIndices[complement] {
                return [complementIndex, i]
            }
            
            // Add the current number and its index to the dictionary
            numIndices[num] = i
        }
        
        // If no solution is found, return an empty array
        return []
    }
}
