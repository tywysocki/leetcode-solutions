---
title: Validate Binary Tree Nodes
desription: A Solution to LeetCode Problem 1361.
icon: material/check
comments: true
tags:
    - Medium
    - Tree
    - Depth First Search
    - Breadth-First Search
    - Union Find
    - Graph
    - Binary Tree
---

## Description

!!! question "LeetCode Problem 1361. Validate Binary Tree Nodes"

    You have `n` binary tree nodes numbered from `0 ` to `n - 1 `where node `i` has two children `leftChild[i]` and `rightChild[i]`, return true if and only if all the given nodes form exactly one valid binary tree. 

    If node`i` has no left child then` leftChild[i]` will equal `-1`, similarly for the right child. 

    Note that the nodes have no values and that we only use the node numbers in this problem.

## Solution

### Intuition

To solve this problem, we need to validate whether the given list of binary tree nodes forms exactly one valid binary tree. A valid binary tree has the following properties:

1. It has one and only one root node.
2. Each non-root node has exactly one parent.
3. There are no cycles in the tree.
4. The tree must be connected.

We'll use these properties to validate the tree.

### Approach

We can approach this problem by simulating the creation of a binary tree while keeping track of the parent of each node. We'll use two arrays: `parent` to store the parent of each node, and `seen` to check if we've seen a node before. The idea is to iterate through the given `leftChild` and `rightChild` arrays and ensure that each child node has only one parent. Additionally, we will check if the tree is connected.

1. Initialize two arrays, `parent` and `seen`, both of size `n`. Initialize `parent` with -1 and `seen` as false for all nodes.

2. Iterate through the `leftChild` and `rightChild` arrays.
    - For each node `i`, check if its left child `leftChild[i]` is not -1 and if we haven't seen it before (i.e., `seen[leftChild[i]]` is false).
        - If true, mark `seen[leftChild[i]]` as true and set `parent[leftChild[i]]` to `i`.
        - If false, return false as this means the same child node has two parents, violating the binary tree property.
    - Repeat the same check for the right child.

3. After iterating through both arrays, check if there is one and only one root node (a node without a parent). Count the number of root nodes by counting the nodes with -1 in the `parent` array.

4. If there is exactly one root node (count of root nodes is 1) and there are no cycles (all nodes are connected and have only one parent), return true. Otherwise, return false.


### Complexity
- Time complexity: $O(n)$ where $n$ is the number of nodes. We iterate through the `leftChild` and `rightChild` arrays once, each taking $O(n)$ time. The depth-first search for connectedness also takes $O(n)$ time.
- Space complexity: $O(n)$ to store the `parent`, `seen`, and `visited` arrays.


### Code
``` { .swift .select }

func validateBinaryTreeNodes(_ n: Int, _ leftChild: [Int], _ rightChild: [Int]) -> Bool {
    var parent = [Int](repeating: -1, count: n)
    var seen = [Bool](repeating: false, count: n)
        
    for i in 0..<n {
        if leftChild[i] != -1 {
            if seen[leftChild[i]] {
                return false // The same child node has two parents.
            }
            seen[leftChild[i]] = true
            parent[leftChild[i]] = i
        }
            
        if rightChild[i] != -1 {
            if seen[rightChild[i]] {
                return false // The same child node has two parents.
            }
            seen[rightChild[i]] = true
            parent[rightChild[i]] = i
        }
    }
        
    var rootCount = 0
    var rootNode = -1
        
    for i in 0..<n {
        if parent[i] == -1 {
            rootCount += 1
            rootNode = i
            if rootCount > 1 {
                 return false // Multiple root nodes.
            }
        }
    }
        
    if rootCount != 1 {
        return false // No root or multiple roots.
    }
        
    // Check if the tree is connected.
    var visited = [Bool](repeating: false, count: n)
        
    func dfs(node: Int) {
        visited[node] = true
        if leftChild[node] != -1 && !visited[leftChild[node]] {
            dfs(node: leftChild[node])
        }
        if rightChild[node] != -1 && !visited[rightChild[node]] {
            dfs(node: rightChild[node])
        }
    }
        
    dfs(node: rootNode)
        
    for i in 0..<n {
        if !visited[i] {
            return false // The tree is not connected.
        }
    }
        
    return true
}
```
