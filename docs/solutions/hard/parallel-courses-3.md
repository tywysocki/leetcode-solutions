---
comments: true

tags:
    - Swift
    - Hard
    - Topological Sorting
    - Graph
---

# Parallell Courses III

## Description

!!! question "LeetCode Problem 2050. Parallel Courses III"

    You are given an integer `n`, which indicates that there are `n` courses labeled from `1` to `n`. You are also given a 2D integer array `relations` where `relations[j] = [prevCoursej, nextCoursej ` denotes that course `prevCoursej` has to be completed before course `nextCoursej` (prerequisite relationship). Furthermore, you are given a **0-indexed** integer array time where `time[i]` denotes how many months it takes to complete the `(i+1)`^th^ course.

    You must find the minimum number of months needed to complete all the courses following these rules:

    - You may start taking a course at any time if the prerequisites are met.
    - Any number of courses can be taken at the same time.

    Return the minimum number of months needed to complete all the courses.

    Note: The test cases are generated such that it is possible to complete every course (i.e., the graph is a directed acyclic graph).


## Solution

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