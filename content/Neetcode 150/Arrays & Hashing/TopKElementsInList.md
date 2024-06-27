---
title: TopKElementsInList
date: June 26, 2024
tags:
  - hashing
  - bucketsort
  - arrays
  - 🍔
  - 🏆
  - sorting
  - heap
---
# Problem

>[!question]
>Given an integer array `nums` and an integer `k`, return the `k` most frequent elements within the array. The test cases are generated such that the answer is always **unique**. You may return the output in **any order**.

**Example 1:**

```java
Input: nums = [1,2,2,3,3,3], k = 2

Output: [2,3]
```

Copy

**Example 2:**

```java
Input: nums = [7,7], k = 1

Output: [7]
```

Copy

**Constraints:**

- `1 <= nums.length <= 10^4`.
- `-1000 <= nums[i] <= 1000`
- `1 <= k <= number of distinct elements in nums`.

---

# Solutions

## 1. Heap Approach

**Time Complexity:** $O(n \log n)$  |  **Space Complexity:** $O(n)$

To solve the problem, we can utilize a heap. First, we count the frequencies of elements using a dictionary, where each key is a number and the value is its frequency. Second, we populate the heap by pushing tuples of frequencies (multiplied by -1, since Python's heap implementation is a minimum heap) and their corresponding keys. Finally, we build the result array by popping the smallest elements from the heap.

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:

        memo = defaultdict(int)
        for num in nums:
            memo[num] += 1

        heap = []
        for key in memo:
            heapq.heappush(heap, (-1 * memo[key], key))

        result = []
        while len(result) < k:
            result.append(heapq.heappop(heap)[1])
        
        return result
```
---
## 2. #🍔  Heap & Python

**Time Complexity:** $O(n \log n)$  |  **Space Complexity:** $O(n)$

Using `Counter` and `heap.nlargest` we can make a #oneliner. 

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
	    return heapq.nlargest(k, Counter(nums).keys(), key=lambda a: Counter(nums)[a])
```

But for a better performance, it's better to perform `Counter` once and store it in the variable. 

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        count = Counter(nums)
        return heapq.nlargest(k, count.keys(), key=lambda x: count[x])
```

---
## 3.  Hashmap & Sorting Approach

**Time Complexity:** $O(n \log n)$  |  **Space Complexity:** $O(n)$

Alternatively, instead of using a `heap`,  we can rely on sorting:

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:

        # count = defaultdict(int)
        # for num in nums:
        #     count[num] += 1

        count = Counter(nums)
        
        sortedNums = sorted(count.items(), key=lambda a: a[1], reverse=True)

        return [num for num, freq in sortedNums[:k]]
```

---
## 4. #🏆  Bucket Sort Approach

**Time Complexity:** $O(n)$  |  **Space Complexity:** $O(n)$

To focus on performance for large inputs, we can solve this problem using the Bucket Sort approach. The key concept here involves creating buckets where each bucket holds numbers whose frequencies are represented by the indexes of the given bucket. For example, if a number occurs only once, it will be placed in the bucket with index `i = 1`. If a number occurs 10 times, it will be located in the bucket with index `i = 10`. The total number of buckets will not exceed the length of the given array `nums`. Once we construct these buckets, we can iterate from the highest index down to populate the `result` array.

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
    
        # count = defaultdict(int)
        # for num in nums:
        #     count[num] += 1
    
        count = Counter(nums)

        buckets = [[] for _ in range(len(nums) + 1)]

        for num, freq in count.items():
            buckets[freq].append(num)
        
        result = []
        for i in range(len(buckets) - 1, -1, -1):
            bucket = buckets[i]
            for el in bucket:
                result.append(el)
                if len(result) == k:
                    return result

        return result
```