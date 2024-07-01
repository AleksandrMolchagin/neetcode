---
title: String Encode and Decode
date: June 30, 2024
tags:
  - strings
  - base64
  - json
  - arrays
---
# Problem

>[!question]
>Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings. Please implement `encode` and `decode`

**Example 1:**

```java
Input: ["neet","code","love","you"]

Output:["neet","code","love","you"]
```

**Example 2:**

```java
Input: ["we","say",":","yes"]

Output: ["we","say",":","yes"]
```

**Constraints:**

- `0 <= strs.length < 100`
- `0 <= strs[i].length < 200`
- `strs[i]` contains only UTF-8 characters.

---
# Solutions

## #🏆 1. Length Prefix

**Time Complexity:** $O(n)$  |  **Space Complexity:** $O(n)$

We can solve the problem by joining all strings together using the integer that represents their lengths followed by a special delimiter (e.g., `#`).

```markdown
["neet","code","love","you"] -> 4#neet4#code4#love3#you
``` 

Once encoded, we decode it by iterating through the string, identifying the length prefix, and using it to extract each substring. This process allows us to reconstruct the original list of strings accurately, ensuring that all characters are correctly handled without ambiguity.
```python
class Solution:
    def encode(self, strs):
        return ''.join(f'{len(s)}#{s}' for s in strs)

    def decode(self, s):
        i = 0
        res = []
        while i < len(s):
            j = i
            while s[j] != '#':
                j += 1
            length = int(s[i:j])
            res.append(s[j+1:j+1+length])
            i = j + 1 + length
        return res
```


---
## #🍔 2. JSON Serialization

**Time Complexity:** $O(n)$  |  **Space Complexity:** $O(n)$

The simplest solution here would be JSON serialization. Using `json.dumps(array)`  we can convert array of strings into a single string representation.:

```python
strs = ["neet", "code", "love", "you"]
s = json.dumps(strs)
print("Result", type(s), s)

#Result <class'str'> ["neet", "code", "love", "you"]
```

Then, using `json.loads(string)` function, we convert it back toe the original array of strings. 

```python
import json

def encode(strs):
    return json.dumps(strs)

def decode(s):
    return json.loads(s)

```

---

## 3.  Base64 Encoding

**Time Complexity:** $O(n)$  |  **Space Complexity:** $O(n)$

Alternatively, we can utilize `Base64` to solve the given problem.

>[!info]
>**Base64** is a binary-to-text encoding scheme that converts binary data into an ASCII string format. It represents binary data using 64 ASCII characters, which makes it suitable for safely transmitting data over text-based protocols such as email or embedding data within URLs. Base64 encoding increases the size of the data by approximately 33% but ensures that the data remains intact without modification during transport.

Since `base64` in Python works only with data in bytes, we need to:

1. Convert the string into bytes using UTF-8 encoding.
2. Encode the byte representation using `base64` 
3. Convert the `base64` encoded bytes back into a readable string format.

```python
import base64
from typing import List

class Solution:
    def encode(self, strs: List[str]) -> str:
        string = ''
        for word in strs:
            w = word.encode()  # b'neet'
            w = base64.b64encode(w)  # b'bmVldA=='
            w = w.decode()  # 'bmVldA=='
            string += w + '\0' 
        return string

    def decode(self, s: str) -> List[str]:
        decoded = [base64.b64decode(word).decode() for word in s.split('\0')[:-1]]
        return decoded

```
