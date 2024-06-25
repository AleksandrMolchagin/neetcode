---
title: Anagram Groups
tags:
  - 🏆
  - hashing
  - primenumber
  - sorting
  - arrays
  - strings
date: June 25, 2024
---
# Anagram Groups


>[!question]
Given an array of strings `strs`, group all _anagrams_ together into sublists. You may return the output in **any order**. An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

**Example 1:**

```java
Input: strs = ["act","pots","tops","cat","stop","hat"]

Output: [["hat"],["act", "cat"],["stop", "pots", "tops"]]
```

**Example 2:**

```java
Input: strs = ["x"]

Output: [["x"]]
```

**Example 3:**

```java
Input: strs = [""]

Output: [[""]]
```


**Constraints:**

- `1 <= strs.length <= 1000`.
- `0 <= strs[i].length <= 100`
- `strs[i]` is made up of lowercase English letters.

---
# Solutions

## Hashing based on a sorted word as a key

**Time Complexity:** $O(n * m \log m)$, where $m$ is the maximum length of a word.
**Space Complexity:** $O(n * m)$, where $m$ is  the maximum length of a word.

To solve the problem, we can iterate through all words and add them to a dictionary that stores all anagrams together. Since anagrams are permutations of each other, sorting them gives the same result. We can group words based on their sorted form. This way, by using the sorted version of each word as the key, we ensure that all anagrams map to the same entry in the dictionary. This approach allows efficient grouping and retrieval of anagram clusters.

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        memo = defaultdict(list)

        for word in strs:
            sortedWord = ''.join(sorted(word))
            memo[sortedWord].append(word)

        result = []
        for key in memo:
            result.append(memo[key])

        return result
```

---
## Hashing based on characters frequencies

Alternatively, we can efficiently group anagrams by counting the occurrences of each character in a word and storing these counts in a fixed-size array of length 26, corresponding to the letters of the English alphabet. After counting the characters, we convert this array into a tuple. This conversion is crucial because tuples, unlike arrays, are immutable and can be used as keys in a dictionary. Using tuples allows us to leverage their hashable property to store and retrieve grouped anagrams reliably.

**Time Complexity:** $O(n * m)$, where $m$ is  an average length of a word.
**Space Complexity:** $O(n * m)$, where $m$ is  an average length of a word.

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        memo = defaultdict(list)

        for word in strs:
            counter = [0] * 26
            for char in word:
                counter[ord(char) - ord('a')] += 1
            memo[tuple(counter)].append(word)

        return memo.values()
```

---
## #🏆  Hashing based on prime number product

**Time Complexity:** $O(n * m)$, where $m$ is  an average length of a word.
**Space Complexity:** $O(n * m)$, where $m$ is  an average length of a word.

We can also leverage prime numbers to generate a unique key for each anagram. Prime numbers are only divisible by one and themselves, ensuring that each product of prime numbers is unique. In this method, each character of the alphabet is assigned a specific prime number. Thus, the product of the primes corresponding to the characters in a word will uniquely identify anagrams, as different combinations of characters will result in different products.

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101]
        memo = defaultdict(list)
        for word in strs:
            key = 1
            for char in word:
                key *= primes[ord(char) - ord('a')]
            memo[key].append(word)
        return memo.values()

```
