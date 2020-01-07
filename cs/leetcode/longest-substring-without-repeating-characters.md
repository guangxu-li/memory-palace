---
description: >-
  https://leetcode.com/problems/longest-substring-without-repeating-characters/
  //TODO: add a page link to hash table.
---

# \* 3. Longest Substring Without Repeating Characters

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

### Approach 2: Sliding Window

#### Algorithm

It is unnecessary to see if a substring has duplicate characters repeatedly. If a substring $$s_{ij}$$ from index $$i$$ to $$j-1$$ is already checked, we only need to check if $$s[j]$$ is already in the substring $$s_{ij}$$.

{% hint style="info" %}
**Tips:**  We can scan the substring which costs $$O(n^2)$$ algorithm, but a better way is using HashSet as a sliding windows which costs $$O(1)$$.
{% endhint %}

{% hint style="success" %}
A window is a range of elements in the array/string which usually defined by the start and end indices, i.e. $$[i,j)$$ \(left-closed, right-open\).

A sliding window is a windows "slides" its two boundaries to the certain direction.
{% endhint %}

1. Use HashSet to store all characters in current window $$[i,j)$$ \($$j=i$$ initially\).
2. Slide the index $$j$$ to the right until $$s[j]$$ is already in the HashSet.
3. Repeat **Step 1** and **Step 2** for all i.

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int ans = 0, i = 0, j = 0;
        while (i < n && j < n) {
            // try to extend the range [i, j]
            if (!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j - i);
            }
            else {
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```

{% hint style="warning" %}
**Note:** `set.remove(s.charAt(i++));` will run multiple times until `s.charAt(i++)` being removed, which can be optimized.
{% endhint %}

#### Complexity Analysis

* **Time complexity:** $$O(2n) = O(n)$$. In the worst case each character will be visited twice by $$i$$ and $$j$$.
* **Space complexity:** $$O(min(m, n))$$. Same as the previous approach.

### Aproach 3: Sliding Window Optimized

We could define a mapping of the characters to its index and skip the characters immediately when we found a repeated character.

{% hint style="info" %}
**i.e.:** If $$s[j]$$ have a duplicate in the range $$[i,j)$$ with index $$j'$$, we can skip all the elements in the range $$[i, j']$$ and let $$i$$ to be $$j'+1$$ directly.
{% endhint %}

#### Java \(Using HashMap\)

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>();
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
}
```

#### Java \(Assuming ASCII 128\)

If we know that the charset is rather small, we can replace the `Map` with an Integer array as direct access table.

{% hint style="info" %}
**Commonly used tables:**

* `int[26]`: Letters 'a' -'z' or 'A' - 'Z'
* `int[128]`: ASCII
* `int[256]`: Extended ASCII
{% endhint %}

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        int[] index = new int[128];
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            i = Math.max(index[s.charAt(j)], i);
            ans = Math.max(ans, j - i + 1);
            index[s.charAt(j)] = j + 1;
        }
        return ans;
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(n)$$. Index $$j$$ will iterate $$n$$ times.
* **Space complexity \(HashMap\):** $$O(min(m,n))$$. Same as the previous approach.
* **Space complexity \(Table\):** $$O(m)$$. $$m$$ is the size of the charset.

