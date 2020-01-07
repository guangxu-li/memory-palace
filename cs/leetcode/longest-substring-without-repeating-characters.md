---
description: 'https://leetcode.com/problems/longest-substring-without-repeating-characters/'
---

# 3. Longest Substring Without Repeating Characters

## Description

Given a string, find the length of the **longest substring** without repeating characters.

**Example 1:**

```text
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```text
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```text
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Solution

### Approach 1: Brute Force

#### Intuition

Check all the substring one by one to see if it has no duplicate character.

#### Algorithm

1. Enumerate all substrings of a given string.
2. If a substring has no duplicate characters, then update the answer.

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j <= n; j++)
                if (allUnique(s, i, j)) ans = Math.max(ans, j - i);
        return ans;
    }

    public boolean allUnique(String s, int start, int end) {
        Set<Character> set = new HashSet<>();
        for (int i = start; i < end; i++) {
            Character ch = s.charAt(i);
            if (set.contains(ch)) return false;
            set.add(ch);
        }
        return true;
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(n^3)$$.

  1. To verify if characters with inex range $$[i,j)$$ are all unique, it costs $$O(j-i)$$ time.
  2. For a given `i`, the sum of time costed by each $$j\in [i+1,n]$$ is $$\sum_{i+1}^{n}O(j-i)$$.

  Thus, the sum of all the time consumption is $$O(\sum_{i=0}^{n-1}(\sum_{j=i+j}^{n}(j-i)))=O(\sum_{i=0}^{n-1}\frac{(1+n-i)(n-i)}{2})$$.

* **Space complexity:** $$O(min(n,m))$$. We need $$O(k)$$ space for checking a substring has no duplicate characters. The maximum value of k depends on the size of string $$n$$ and the size of the charset/alphabet $$m$$.

