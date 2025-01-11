---
title: Count Number of Islands
date: July 28, 2024
tags:
  - dfs
  - bfs
---
## Problem

Given a 2D grid `grid` where `'1'` represents land and `'0'` represents water, count and return the number of islands.

An **island** is formed by connecting adjacent lands horizontally or vertically and is surrounded by water. You may assume water is surrounding the grid (i.e., all the edges are water).

**Example 1:**

```java
Input: grid = [
    ["0","1","1","1","0"],
    ["0","1","0","1","0"],
    ["1","1","0","0","0"],
    ["0","0","0","0","0"]
  ]
Output: 1
```

Copy

**Example 2:**

```java
Input: grid = [
    ["1","1","0","0","1"],
    ["1","1","0","0","1"],
    ["0","0","1","0","0"],
    ["0","0","0","1","1"]
  ]
Output: 4
```

Copy

**Constraints:**

- `1 <= grid.length, grid[i].length <= 100`
- `grid[i][j]` is `'0'` or `'1'`.

---


## Solutions

## 1. DFS Traversal

**Time Complexity:** $O(N * M)$  |  **Space Complexity:** $O(N * M)$

To solve the problem, we traverse the grid using a DFS approach. The goal is to mark islands as visited by storing visited items in a separate set.

```python

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:

        visited = set()

        def dfs(x, y):

            if x >= len(grid) or x < 0 or y >= len(grid[0]) or y < 0:
                return

            if (x, y) in visited or grid[x][y] == '0':
                return

            visited.add((x, y))
            
            nei = [(x + 1, y), (x - 1, y), (x, y + 1), (x, y - 1)]

            for new_x, new_y in nei:
                dfs(new_x, new_y)

        counter = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1' and (i, j) not in visited:
                    dfs(i, j)
                    counter += 1
        
        return counter

```

The solution can be improved further. Instead of relying on a set to mark visited locations, we can modify the given grid to mark them as `0`.

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:

        def dfs(x, y):

            if x >= len(grid) or x < 0 or y >= len(grid[0]) or y < 0:
                return

            if grid[x][y] == '0':
                return

            grid[x][y] = '0'
        
            nei = [(x + 1, y), (x - 1, y), (x, y + 1), (x, y - 1)]

            for new_x, new_y in nei:
                dfs(new_x, new_y)

        counter = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    dfs(i, j)
                    counter += 1
        
        return counter
```

---
## 2. BFS Traversal

**Time Complexity:** $O(N * M)$  |  **Space Complexity:** $O(N * M)$

Alternatively, we can use BFS approach.

```python

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:

        def bfs(x, y):

            queue = deque([(x, y)])

            while queue:
                cx, cy = queue.popleft()
                nei = [(cx + 1, cy), (cx - 1, cy), (cx, cy + 1), (cx, cy - 1)]

                for nx, ny in nei:

                    if nx >= len(grid) or nx < 0 or ny >= len(grid[0]) or ny < 0:
                        continue
                    if grid[nx][ny] == '0':
                        continue

                    grid[nx][ny] = '0'
                    queue.append((nx, ny))

        counter = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    bfs(i, j)
                    counter += 1
        
        return counter
```