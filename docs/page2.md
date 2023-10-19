# **Featured Solutions**

## Validate Binary Tree Nodes

!!! question "Problem Description"

    You have `n` binary tree nodes numbered from `0 ` to `n - 1 `where node `i` has two children `leftChild[i]` and `rightChild[i]`, return true if and only if all the given nodes form exactly one valid binary tree. 

    If node`i` has no left child then` leftChild[i]` will equal `-1`, similarly for the right child. 

    Note that the nodes have no values and that we only use the node numbers in this problem.

### Solution

/// tab | Intuition
    new: true
    select: true

To solve this problem, we need to validate whether the given list of binary tree nodes forms exactly one valid binary tree. A valid binary tree has the following properties:

1. It has one and only one root node.
2. Each non-root node has exactly one parent.
3. There are no cycles in the tree.
4. The tree must be connected.

We'll use these properties to validate the tree.
///
    
/// tab | Approach
We can approach this problem by simulating the creation of a binary tree while keeping track of the parent of each node. We'll use two arrays: `parent` to store the parent of each node, and `seen` to check if we've seen a node before. The idea is to iterate through the given `leftChild` and `rightChild` arrays and ensure that each child node has only one parent. Additionally, we will check if the tree is connected.

1. Initialize two arrays, `parent` and `seen`, both of size `n`. Initialize `parent` with -1 and `seen` as false for all nodes.

2. Iterate through the `leftChild` and `rightChild` arrays.
    - For each node `i`, check if its left child `leftChild[i]` is not -1 and if we haven't seen it before (i.e., `seen[leftChild[i]]` is false).
        - If true, mark `seen[leftChild[i]]` as true and set `parent[leftChild[i]]` to `i`.
        - If false, return false as this means the same child node has two parents, violating the binary tree property.
    - Repeat the same check for the right child.

3. After iterating through both arrays, check if there is one and only one root node (a node without a parent). Count the number of root nodes by counting the nodes with -1 in the `parent` array.

4. If there is exactly one root node (count of root nodes is 1) and there are no cycles (all nodes are connected and have only one parent), return true. Otherwise, return false.
///

/// tab | Complexity
- Time complexity: **O(n)** where n is the number of nodes. We iterate through the `leftChild` and `rightChild` arrays once, each taking **O(n)** time. The depth-first search for connectedness also takes **O(n)** time.
- Space complexity: **O(n)** to store the `parent`, `seen`, and `visited` arrays.
///

/// tab | Code
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
///

---

## Parallell Courses III

!!! question "Problem Description"

    You are given an integer `n`, which indicates that there are `n` courses labeled from `1` to `n`. You are also given a 2D integer array `relations` where `relations[j] = [prevCoursej, nextCoursej ` denotes that course `prevCoursej` has to be completed before course `nextCoursej` (prerequisite relationship). Furthermore, you are given a **0-indexed** integer array time where `time[i]` denotes how many months it takes to complete the `(i+1)`^th^ course.

    You must find the minimum number of months needed to complete all the courses following these rules:

    - You may start taking a course at any time if the prerequisites are met.
    - Any number of courses can be taken at the same time.

    Return the minimum number of months needed to complete all the courses.

    Note: The test cases are generated such that it is possible to complete every course (i.e., the graph is a directed acyclic graph).


### Solution

/// tab | Intuition
    new: true
    select: true

To solve this problem, we can use a topological sorting approach. We'll build a directed graph representing the course prerequisites and track the time required to complete each course. By iteratively selecting courses that have all their prerequisites met, we can minimize the time needed to complete all courses.
///

/// tab | Approach
1. Create a graph representation of course prerequisites using an adjacency list. Initialize an array `inDegree` to store the in-degrees of each course, and an array `courseTime` to store the time required to complete each course.

2. Populate the graph based on the relations provided in the input.

3. Initialize a queue for topological sorting and add all courses with no prerequisites to the queue. These are the courses we can start immediately.

4. Initialize a variable `completedCourses` to keep track of the number of courses completed and `months` to keep track of the total time spent.

5. While the queue is not empty, do the following:
- Dequeue a course from the queue.
- Update `completedCourses` and `months` accordingly.
- For each course that can be taken after completing the dequeued course (based on the graph), decrement its in-degree by 1.
- Consider the maximum time for the next course after completing its prerequisites.
- If the in-degree of the course becomes 0, add it to the queue as it can now be taken.

6. After the loop, if `completedCourses` is equal to `n`, we have completed all courses, and the value of `months` is the minimum time required. Return `months`.

7. If `completedCourses` is less than `n`, it means there's a cycle in the course dependencies, and it's not possible to complete all courses. Return -1 in this case.
///

/// tab | Complexity

- Time complexity: O(n + m), where n is the number of courses and m is the number of prerequisite relationships.
- Space complexity: O(n), where n is the number of courses.
///

/// tab | Code
``` { .swift .select }
class Solution {
    func minimumTime(_ n: Int, _ relations: [[Int]], _ time: [Int]) -> Int {
        // Create the graph and initialize in-degrees
        var graph = [Int: [Int]]()
        var inDegree = [Int](repeating: 0, count: n)
        var courseTime = [Int](repeating: 0, count: n)
            
        for relation in relations {
            let prevCourse = relation[0] - 1
            let nextCourse = relation[1] - 1
            if graph[prevCourse] == nil {
                graph[prevCourse] = [nextCourse]
            } else {
                graph[prevCourse]?.append(nextCourse)
            }
            inDegree[nextCourse] += 1
        }
            
        // Initialize the queue with courses having no prerequisites
        var queue = [Int]()
        for course in 0..<n {                if inDegree[course] == 0 {
                queue.append(course)
            }
        }
            
        var completedCourses = 0
        var months = 0
            
        while !queue.isEmpty {
            let currentCourse = queue.removeFirst()
            completedCourses += 1
            months = max(months, courseTime[currentCourse] + time[currentCourse])
                
            if let nextCourses = graph[currentCourse] {
                for nextCourse in nextCourses {
                    inDegree[nextCourse] -= 1
                    courseTime[nextCourse] = max(courseTime[nextCourse], courseTime[currentCourse] + time[currentCourse])
                        
                    if inDegree[nextCourse] == 0 {
                        queue.append(nextCourse)
                    }
                }
            }
        }
            
        if completedCourses == n {
            return months
        }
            
        // In case of a cycle, it's not possible to complete all courses.
        return -1
    }
}
```
///