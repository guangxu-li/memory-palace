---
description: Easy
---

# C 167. Two Sum II - Input array is sorted

## [Description](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

Given an array of integers that is already _**sorted in ascending order**_, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where **index1** must be less than **index2**.

**Note:**

* Your returned answers \(both **index1** and **index2**\) are not zero-based.
* You may assume that each input would have _exactly_ one solution and you may not use the _same_ element twice.

**Example:**

```text
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

## Approach: Two Pointers

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int lo = 0;
        int hi = numbers.length - 1;

        while (lo < hi) {
            int sum = numbers[lo] + numbers[hi];

            if (sum == target) {
                return new int[] {lo + 1, hi + 1};
            } else if (sum > target) {
                hi--;
            } else {
                lo++;
            }
        }

        return new int[] {-1, -1};
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(n)$$.
* **Space complexity:** $$O(1)$$.

