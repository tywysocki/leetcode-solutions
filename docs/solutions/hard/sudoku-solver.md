---
title: Sudoku Solver
description: A solution to LeetCode problem 37
icon: material/check
comments: true
tags:
    - Hard
    - Swift
    - Array
    - Hash Table
    - Back Tracking
    - Matrix
---

## Description

!!! question "LeetCode Problem 37. Sudoku Solver"

    Write a program to solve a Sudoku puzzle by filling the empty cells.

    A sudoku solution must satisfy all of the following rules:

    1. Each of the digits `1-9` must occur exactly once in each row.
    2. Each of the digits `1-9` must occur exactly once in each column.
    3. Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

    The `'.'` character indicates empty cells.

## Solution

The Sudoku puzzle can be solved using a backtracking algorithm. We'll start by iterating through the entire board, looking for empty cells. For each empty cell, we'll try to fill it with numbers from '1' to '9' while ensuring that the Sudoku rules are satisfied. If we reach an invalid state, we'll backtrack and try a different number until a valid solution is found or it's determined that the puzzle is unsolvable.

## Approach

1. Create a function `solveSudoku` that takes a Sudoku board as input.

2. Inside the `solveSudoku` function, call the `solve` function to solve the puzzle.

3. The `solve` function is the heart of the backtracking algorithm. It iterates through the entire board, looking for empty cells. When it finds an empty cell, it tries to fill it with numbers from '1' to '9'.

4. For each number, it checks if it's a valid move according to the Sudoku rules using the `isValid` function.

5. If a number is valid, it fills the cell with that number and recursively calls the `solve` function. If the recursion is successful (i.e., the puzzle is solved), it returns true.

6. If the recursion fails, it means that the current number didn't lead to a valid solution, so it backtracks by setting the cell back to `.` and continues trying other numbers.

7. The `isValid` function checks if a number is valid in the current row, column, and 3x3 sub-grid according to the Sudoku rules.

## Complexity

### Time complexity:
The backtracking algorithm explores possible solutions, and in the worst case, it might try all possibilities, which is exponential. So, the time complexity is $O(9^{n^{2}})$, where $n$ is the size of the Sudoku board (usually 9x9).

### Space complexity:
The space complexity is $O(n^{2})$ to store the Sudoku board, where n is the size of the board. The recursive call stack also contributes to space complexity, but it's bounded by the size of the board, so it's $O(n^{2})$.

## Code

```swift
class Solution {
    func solveSudoku(_ board: inout [[Character]]) {
        solve(&board)
    }
    
    func solve(_ board: inout [[Character]]) -> Bool {
        for i in 0..<9 {
            for j in 0..<9 {
                if board[i][j] == "." {
                    for num in "123456789" {
                        if isValid(board, i, j, num) {
                            board[i][j] = num
                            if solve(&board) {
                                return true
                            }
                            board[i][j] = "."
                        }
                    }
                    return false
                }
            }
        }
        return true
    }
    
    func isValid(_ board: [[Character]], _ row: Int, _ col: Int, _ num: Character) -> Bool {
        for i in 0..<9 {
            if board[row][i] == num || board[i][col] == num || board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == num {
                return false
            }
        }
        return true
    }
}
```