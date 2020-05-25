---
description: Easy
---

# C 1. Two Sum

## [Description](https://leetcode.com/problems/two-sum/)

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have _**exactly**_ one solution, and you may not use the _same_ element twice.

**Example:**

```text
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## Solution

### Approach 1: Brute Force

The brute force approach is simple. Loop through each element $$x$$ and find if there is another value that equals to $$target - x$$.

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] == target - nums[i]) {
                    return new int[] { i, j };
                }
            }
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(n^2)$$. For each element, we try to find its complement by looping through the rest of array which takes $$O(n)$$time. Therefore, the time complexity is $$O(n^2)$$.
* **Space complexity:** $$O(1)$$.

### Approach 2: Two-pass Hash Table

The best way to maintain a mapping of each element in the array to its index is a hash table. To improve the performance, change the array to a hash table.

{% hint style="success" %}
A hash table is built to reduce look up time from $$O(n)$$ to $$O(1)$$ by trading space for speed.
{% endhint %}

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement) && map.get(complement) != i) {
                return new int[] { i, map.get(complement) };
            }
        }
        throw new IllegalArgumentException("No two sum solution");
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(n)$$. For each element, we search its complement in the table which takes $$O(1)$$time.
* **Space complexity:** $$O(n)$$. The hash table stores n elements.

### Approach 3: One-pass Hash Table

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];

            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };
            } else {
                map.put(nums[i], i);
            }
        }

        throw new IllegalArgumentException("No Answer.");
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(n)$$. For each element, we search its complement in the table which takes $$O(1)$$time.
* **Space complexity:** $$O(n)$$. The hash table stores at most n elements.

## Related Topics

[_Array_](https://leetcode.com/tag/Array/)_,_ [_Hash Table_](https://leetcode.com/tag/hash-table/)\_\_

| Similar Questions | Difficulty |
| :--- | :--- |
| [3Sum](3sum.md) | _Medium_ |
| [4Sum](4sum.md) | _Medium_ |
| [Two Sum II - Input array is sorted](../101-200/two-sum-ii-input-array-is-sorted.md) | Easy |
| [Two Sum III - Data structure design](../101-200/two-sum-iii-data-structure-design.md) | Easy |
| [Subarray Sum Equals K](../501-600/subarray-sum-equals-k.md) | _Medium_ |
| [Two Sum IV - Input is a BST](../601-700/two-sum-iv-input-is-a-bst.md) | Easy |
| [Two Sum Less Than K](../1001-1100/two-sum-less-than-k.md) | Easy |
