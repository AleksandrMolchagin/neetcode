---
title: Valid Sudoku
date: July 4, 2024
tags:
  - bitmasking
  - hashing
  - arrays
---
# Problem

You are given a a `9 x 9` Sudoku board `board`. A Sudoku board is valid if the following rules are followed:

1. Each row must contain the digits `1-9` without duplicates.
2. Each column must contain the digits `1-9` without duplicates.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without duplicates.

Return `true` if the Sudoku board is valid, otherwise return `false`

Note: A board does not need to be full or be solvable to be valid.

**Example 1:**

![[2_M-SpV7SZTmw9YHC734tSHQ.avif]]

```java
Input: board = 
[["1","2",".",".","3",".",".",".","."],
 ["4",".",".","5",".",".",".",".","."],
 [".","9","8",".",".",".",".",".","3"],
 ["5",".",".",".","6",".",".",".","4"],
 [".",".",".","8",".","3",".",".","5"],
 ["7",".",".",".","2",".",".",".","6"],
 [".",".",".",".",".",".","2",".","."],
 [".",".",".","4","1","9",".",".","8"],
 [".",".",".",".","8",".",".","7","9"]]

Output: true
```

**Example 2:**

```java
Input: board = 
[["1","2",".",".","3",".",".",".","."],
 ["4",".",".","5",".",".",".",".","."],
 [".","9","1",".",".",".",".",".","3"],
 ["5",".",".",".","6",".",".",".","4"],
 [".",".",".","8",".","3",".",".","5"],
 ["7",".",".",".","2",".",".",".","6"],
 [".",".",".",".",".",".","2",".","."],
 [".",".",".","4","1","9",".",".","8"],
 [".",".",".",".","8",".",".","7","9"]]

Output: false
```

Explanation: There are two 1's in the top-left 3x3 sub-box.

**Constraints:**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit `1-9` or `'.'`

---

# Solutions

## 1. Hashing

**Time Complexity:** $O(1)$  |  **Space Complexity:** $O(1)$

We can solve the problem by iterating through all rows and columns, and adding elements to related dictionaries. If we encounter that an element is already present in its dictionary, we return `false`. In total, we will have 9 column dictionaries, 9 row dictionaries, and 9 sub-box dictionaries. To calculate the related dictionary for a sub-box, we divide `row` and `col` by 3, as it will round the number, making it an integer.

```python

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        
        cols = defaultdict(set)
        rows = defaultdict(set)
        subboxes = defaultdict(set)

        for row in range(len(board)):
            for col in range(len(board[0])):

                val = board[row][col]
                if val == ".":
                    continue

                if val in cols[col]:
                    return False
                if val in rows[row]:
                    return False
                if val in subboxes[(row // 3, col // 3)]:
                    return False

	            
                cols[col].add(val)
                rows[row].add(val)
                subboxes[(row // 3, col // 3)].add(val)
                
        return True
```

---
## 2. Arrays

**Time Complexity:** $O(1)$  |  **Space Complexity:** $O(1)$

Alternatively, we can utilize lists to keep track of elements.

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:

        cols = defaultdict(list)
        rows = defaultdict(list)
        subboxes = defaultdict(list)

        for row in range(len(board)):
            for col in range(len(board[0])):

                val = board[row][col]
                if val == ".":
                    continue

                if val in cols[col]:
                    return False
                if val in rows[row]:
                    return False
                if val in subboxes[(row // 3, col // 3)]:
                    return False

                cols[col].append(val)
                rows[row].append(val)
                subboxes[(row // 3, col // 3)].append(val)
                
        return True

```

---
## #🏆  3. Bitmasking

**Time Complexity:** $O(1)$  |  **Space Complexity:** $O(1)$

To improve our solution further and reduce the amount of space used, we use bitmasking. By representing the presence of each number in rows, columns, and sub-boxes with bits in an integer, we can efficiently check and mark the existence of numbers.

To check if a number is already present in a row, column, or sub-box, we use the bitwise AND operation: `cols[col] & (1 << val)`

- `1 << val` creates a bitmask with only the `val`-th bit set to 1
- `cols[col] & (1 << val)` checks if the `val`-th bit is set in `cols[col]`. If it is non-zero, the number is already present

To mark a number as present, we use the bitwise OR operation: `cols[col] |= (1 << val)`

- `1 << val` creates a bitmask with only the `val`-th bit set to 1
- `cols[col] |= (1 << val)` sets the `val`-th bit in `cols[col]`, marking the number as present

```python
from typing import List

class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:

        cols = [0] * 9
        rows = [0] * 9
        subboxes = [0] * 9

        for row in range(len(board)):
            for col in range(len(board[0])):

                if board[row][col] == ".":
                    continue

                val = int(board[row][col]) - 1
                sb = (row // 3) * 3 + (col // 3)

                if cols[col] & (1 << val):
                    return False
                if rows[row] & (1 << val):
                    return False
                if subboxes[sb] & (1 << val):
                    return False
                
                cols[col] |= (1 << val)
                rows[row] |= (1 << val)
                subboxes[sb] |= (1 << val)
                            
        return True

```
