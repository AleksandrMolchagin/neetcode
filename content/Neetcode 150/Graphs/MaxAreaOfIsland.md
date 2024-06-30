---
title: Max Area of Island
tags:
  - bfs
  - dfs
  - unionfind
---
# Problem

>[!question] 
>You are given a matrix `grid` where `grid[i]` is either a `0` (representing water) or `1` (representing land). An island is defined as a group of `1`'s connected horizontally or vertically. You may assume all four edges of the grid are surrounded by water. The **area** of an island is defined as the number of cells within the island. Return the maximum **area** of an island in `grid`. If no island exists, return `0`.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/8eeb491c-c8ff-4ed6-78ed-ce4cf87d7200/public)

```java
Input: grid = [
  [0,1,1,0,1],
  [1,0,1,0,1],
  [0,1,1,0,1],
  [0,1,0,0,1]
]

Output: 6
```

Explanation: `1`'s cannot be connected diagonally, so the maximum area of the island is `6`.

**Constraints:**

- `1 <= grid.length, grid[i].length <= 50`

---

# Solutions

# 1.  #🏆  Depth-First Search

**Time Complexity:** $O(n)$  |  **Space Complexity:** $O(n)$

We can solve the problem using Depth-First Search (DFS). This method explores each cell in the grid with a value of 1 that hasn't been visited. DFS recursively visits all connected land cells to calculate the total area of an island by following these steps:

1. If the cell is out of bounds, return 0.
2. If the cell is already visited or is water, return 0.
3. Otherwise, explore all neighboring cells.

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        visited = set()

        def dfs(x, y):
            if x < 0 or y < 0 or x >= len(grid) or y >= len(grid[0]):
                return 0
            if (x, y) in visited or grid[x][y] == 0:
                return 0
            visited.add((x, y))
            
            area = 1
            area += dfs(x + 1, y)
            area += dfs(x - 1, y)
            area += dfs(x, y + 1)
            area += dfs(x, y - 1)

            return area

        maximum = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1 and (i, j) not in visited:
                    maximum = max(maximum, dfs(i, j))
        
        return maximum

```

---
# 2. Breadth-First Search

**Time Complexity:** $O(n)$  |  **Space Complexity:** $O(n)$

A similar solution is achieved through BFS:

1. Start from an unvisited land cell and add it to the queue.
2. For each cell, check if it's out of bounds; if so, skip it.
3. Otherwise, mark it as visited, count it, and add all unvisited neighbors to the queue. 
4. Keep traversing through the queue.

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        visited = set()
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        def bfs(x, y):
            queue = deque([(x, y)])
            visited.add((x, y))
            area = 0

            while queue:
                cx, cy = queue.popleft()
                area += 1

                for dx, dy in directions:
                    nx, ny = cx + dx, cy + dy
                    if 0 <= nx < len(grid) and 0 <= ny < len(grid[0]) and (nx, ny) not in visited and grid[nx][ny] == 1:
                        visited.add((nx, ny))
                        queue.append((nx, ny))

            return area

        maximum = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1 and (i, j) not in visited:
                    maximum = max(maximum, bfs(i, j))

        return maximum

```

---
# 3. Union-Find

**Time Complexity:** $O(n)$  |  **Space Complexity:** $O(n)$

Using a Union-Find data structure, also known as Disjoint Set Union (DSU), we can connect all islands together in a graph, while keeping their areas in the supplemental `area` dictionary. 

To understand the Union-Find data structure, refer to the [video](https://www.youtube.com/watch?v=ayW5B2W9hfo) .

The `find` operation helps identify the root of each set, while `union` merges two sets and updates their areas, using path compression and union by rank for optimal performance. What is a path compression? Path compression is an optimization technique that flattens the structure of the tree whenever `find` is called, by making nodes point directly to the root.

![[assets/1*M-SpV7SZTmw9YHC734tSHQ.png]]


```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        parent = {}
        rank = {}
        area = {}

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(x, y):
            rootX = find(x)
            rootY = find(y)

            if rootX != rootY:
                if rank[rootX] > rank[rootY]:
                    parent[rootY] = rootX
                    area[rootX] += area[rootY]
                elif rank[rootX] < rank[rootY]:
                    parent[rootX] = rootY
                    area[rootY] += area[rootX]
                else:
                    parent[rootY] = rootX
                    rank[rootX] += 1
                    area[rootX] += area[rootY]

        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 1:
                    parent[(i, j)] = (i, j)
                    rank[(i, j)] = 0
                    area[(i, j)] = 1

        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 1:
                    for ni, nj in [(i + 1, j), (i - 1, j), (i, j + 1), (i, j - 1)]:
                        if 0 <= ni < rows and 0 <= nj < cols and grid[ni][nj] == 1:
                            union((i, j), (ni, nj))

        return max(area.values(), default=0)

```