---
title: Contains Duplicate
tags:
  - arrays
  - hashing
  - sorting
  - binarysearchtree
date: June 21, 2024
---
# Problem

> [!question] 
> 
> Given an integer array `nums`, return `true` if any value appears **more than once** in the array, otherwise return `false`.

**Example 1:**

```java
Input: nums = [1, 2, 3, 3]

Output: true
```

**Example 2:**

```java
Input: nums = [1, 2, 3, 4]

Output: false
```

---
# Solutions

## 1. Brute Force

**Time Complexity:** $O(n^2)$  |  **Space Complexity:** $O(1)$ 

In the brute force approach, we iterate through all the numbers in the array and compare each number with every other number. This method ensures that we check all possible pairs to determine if there are any duplicates.

```python
def hasDuplicate(self, nums: List[int]) -> bool:
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] == nums[j]:
                return True
    return False
```
---
## 2. Sorting

**Time Complexity:** $O(n \log n)$  |  **Space Complexity:** $O(1)$ 

The sorting approach involves sorting the array first and then checking for adjacent duplicate elements. This method leverages the fact that any duplicate elements will be next to each other after sorting.

```python
def hasDuplicate(self, nums: List[int]) -> bool:
    nums.sort()
    for i in range(1, len(nums)):
        if nums[i] == nums[i - 1]:
            return True
    return False
```

---
## 3. #🏆 Hash Set 

**Time Complexity:** $O(n \log n)$  |  **Space Complexity:** $O(n)$ 

The hash map approach uses a hash map (or dictionary in Python) to count the occurrences of each element in the array. If any element occurs more than once, it means there are duplicates.

```python
def hasDuplicate(self, nums: List[int]) -> bool:
    seen = set()
    for num in nums:
        if num in seen:
            return True
        seen.add(num)
    return False
```

---
## 4.  #🍔 Python & Set

**Time Complexity:** $O(n)$  |  **Space Complexity:** $O(n)$ 

Python-based elegant #oneliner. In this approach, we leverage Python's `set` data structure to determine if there are duplicates in the array. 

```python
def hasDuplicate(self, nums: List[int]) -> bool:
    return len(nums) != len(set(nums))
```

---
## 5. Binary Search Tree

**Time Complexity:** $O(n \log n)$ on average, to $O(n^2)$ - worst   

**Space Complexity:** $O(n)$ 

Overkill, but we can solve the problem by building a Binary Search Tree since a [standard BST typically does not allow duplicate values](https://stackoverflow.com/questions/300935/are-duplicate-keys-allowed-in-the-definition-of-binary-search-trees). Building a BST is simple: if a `value` is less than the `root's value`, traverse to the left subtree; if greater, traverse to the right subtree. If a `value` is equal to the `root's value`, a duplicate is detected!

```python
class TreeNode:
    def __init__(self, key):
        self.left = None
        self.right = None
        self.val = key

def insert(root, key):
    if root is None:
        return (True, TreeNode(key))
    else:
        if root.val < key:
            inserted, root.right = insert(root.right, key)
        elif root.val > key:
            inserted, root.left = insert(root.left, key)
        elif root.val == key:
            return (False, root)
    return (True, root)

class Solution:
    def hasDuplicate(self, nums: list[int]) -> bool:
        root = None
        for num in nums:
            inserted, root = insert(root, num)
            if not inserted:
                return True
        return False
```

---
## 6.  #🎈 Bit Manipulation 

Fast, yet limited.


---
## 7.  #🎈 Floyd's Tortoise and Hare

Fun, but not practical.




