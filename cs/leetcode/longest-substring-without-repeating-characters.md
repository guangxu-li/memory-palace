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

```