---
title: Backspace String Compare
description: A Solution to LeetCode Problem 844
icon: material/check
comments: true
tags:
  - Easy
  - Swift
  - Two Pointers
  - String
  - Stack 
  - Simulation
---

## Description

Given two strings `#!swift s` and `#!swift t`, return `#!swift true` if they are equal when both are typed into empty text editors. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

=== "Example 1"

    ``` { .markdown title="" }
    Input: s = "ab#c", t = "ad#c"
    Output: true
    Explanation: Both s and t become "ac".
    ```
=== "Example 2"

    ``` { .markdown title="" }
    Input: s = "ab##", t = "c#d#"
    Output: true
    Explanation: Both s and t become "".
    ```
=== "Example 3"

    ``` { .markdown title="" }
    Input: s = "a#c", t = "b"
    Output: false
    Explanation: s becomes "c" while t becomes "b".
    ```

---

## Solution

Backspace String Compare using Stack-Based Simulation

### Intuition

To solve this problem, we can simulate the typing process for both strings `s` and `t` by applying the backspace actions and then comparing the resulting strings.

### Approach

1. Create two helper functions: one for processing a string with backspaces and one for converting the processed string into the final string.

2. In the `processString` function, initialize an empty array to represent the stack. Iterate through each character in the input string. If the character is not a backspace ('#'), push it onto the stack. If it is a backspace, pop an element from the stack if the stack is not empty.

3. After processing both strings `s` and `t`, apply the `convertToString` function to both processed strings to get their final representations.

4. Finally, compare the two final strings to check if they are equal. Return `true` if they are equal, and `false` otherwise.

### Complexity

- Time complexity: $O(n)$, where $n$ is the length of the longer string between `s` and `t`. We iterate through both strings once.
- Space complexity: $O(n)$, as we use a stack to store the processed characters.

### Code

```swift
class Solution {
    func backspaceCompare(_ s: String, _ t: String) -> Bool {
        func processString(_ str: String) -> [Character] {
            var stack = [Character]()
            for char in str {
                if char != "#" {
                    stack.append(char)
                } else if !stack.isEmpty {
                    stack.removeLast()
                }
            }
            return stack
        }
        
        func convertToString(_ arr: [Character]) -> String {
            return String(arr)
        }
        
        let processedS = convertToString(processString(s))
        let processedT = convertToString(processString(t))
        
        return processedS == processedT
    }
}
```