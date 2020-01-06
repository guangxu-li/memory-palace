---
description: 'https://leetcode.com/problems/two-sum/ //TODO: Add Hash Table link'
---

# \*1. Two Sum

## Description

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have _**exactly**_ one solution, and you may not use the _same_ element twice.

**Example:**

```text
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## Solution

### Brute Force

The brute force approach is simple. Loop through each element $$x$$ and find if there is another value that equals to $$target - x$$.

#### Complexity Analysis

* **Time complexity:** $$O(n^2)$$ . For each element, we try to find its complement by looping through the rest of array which takes $$O(n)$$time. Therefore, the time complexity is $$O(n^2)$$.
* **Space complexity:** $$O(1)$$.

```java
class solution {
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

### One-pass Hash Table

The best way to maintain a mapping of each element in the array to its index is a hash table. To improve the performance, change the array to a hash table.

{% hint style="success" %}
A hash table is built to reduce look up time from $$O(n)$$ to $$O(1)$$ by trading space for speed.
{% endhint %}

#### Complexity Analysis

* **Time complexity:** $$O(n)$$ . For each element, we search its complement in the table which takes $$O(1)$$time.
* **Space complexity:** $$O(n)$$. The hash table stores at most n elements.

```java
class solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            
            if (map.containsKey(complement)) {
                return new int[] {map.get(complement), i};
            }
            
            map.put(nums[i], i);
        }
        
        throw new IllegalArgumentException("No Answer.");
    }
}
```



