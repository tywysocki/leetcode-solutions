---
title: Add Two Numbers
description: A solution to LeetCode problem 2
icon: material/check
comments: true
tags:
    - Medium
    - Linked List
    - Recursion
    - Math
---


# Adding Two Numbers as Linked Lists

## Description

!!! question "LeetCode Problem 2: Add Two Numbers"
    You are given two linked lists, each representing a non-negative integer. The digits are stored in reverse order, meaning that the ones digit is at the head of the list. You need to add these two numbers and return the result as a linked list. 


## Intuition
To add two numbers as linked lists, we need to simulate the addition process, just like we do manually on paper. We start from the heads of both lists, add the corresponding digits along with any carry from the previous step, and record the result as a new node in the result list. If the sum of two digits is greater than 9, we carry over the extra digit to the next step.

## Approach

1. Initialize three pointers: `l1` pointing to the head of the first list, `l2` pointing to the head of the second list, and `dummyHead` pointing to a dummy node. Also, initialize variables `carry` to 0 and `current` to `dummyHead`.
2. Traverse both lists simultaneously while there are nodes to process in either list or there is a carry.
3. For each step, calculate the sum of the current nodes' values along with the carry from the previous step.
4. Create a new node with the value being the sum % 10 (to handle the carry).
5. Update the carry for the next step (sum / 10).
6. Move `current` to the new node.
7. Move `l1` and `l2` to their next nodes if available.
8. After the loop, if there is a remaining carry, create a final node with its value and append it to the result list.
9. Return `dummyHead.next`, which is the head of the resulting linked list.

## Complexity

### Time Complexity
- We traverse both linked lists once, so the time complexity is $O(max(N, M))$, where $N$ and $M$ are the lengths of the input linked lists.

### Space Complexity
- We use a few extra pointers and variables, but the space complexity is $O(max(N, M))$ as we need to store the result.

## Code

```swift
class Solution {
    func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        var l1 = l1
        var l2 = l2
        var carry = 0
        let dummyHead = ListNode(0)
        var current: ListNode? = dummyHead
        
        while l1 != nil || l2 != nil {
            let x = l1?.val ?? 0
            let y = l2?.val ?? 0
            let sum = x + y + carry
            carry = sum / 10
            current?.next = ListNode(sum % 10)
            current = current?.next
            if l1 != nil { l1 = l1?.next }
            if l2 != nil { l2 = l2?.next }
        }
        
        if carry > 0 {
            current?.next = ListNode(carry)
        }
        
        return dummyHead.next
    }
}
```