---
description: Medium
---

# B 560. Subarray Sum Equals K

## [Description](https://leetcode.com/problems/subarray-sum-equals-k/)

Given an array of integers and an integer **k**, you need to find the total number of continuous subarrays whose sum equals to **k**.

**Example 1:**

```text
Input:nums = [1,1,1], k = 2
Output: 2
```

**Note:**

1. The length of the array is in range \[1, 20,000\].
2. The range of numbers in the array is \[-1000, 1000\] and the range of the integer **k** is \[-1e7, 1e7\].

## Approach 1: Cumulative Sum

### Intuition

Let `sum[i]` represents the sum from `nums[0]` to `nums[i - 1]`.

Then the sum of subarray `nums[i], nums[i + 1], ..., nums[j]` is `sum[j + 1] - sum[i]`.

### Implementation

#### With extra space

{% tabs %}
{% tab title="Java" %}
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> sums = new HashMap<Integer, Integer>();
        sums.put(0, 1);

        int count = 0;
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            count += sums.getOrDefault(sum - k, 0);
            sums.put(sum, sums.getOrDefault(sum, 0) + 1);
        }

        return count;
    }
}
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

#### Without extra space

{% tabs %}
{% tab title="Java" %}
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;

        for (int i = 0; i < nums.length; i++) {
            int sum = 0;

            for (int j = i; j < nums.length; j++) {
                sum += nums[j];

                if (sum == k) {
                    count++;
                }
            }
        }

        return count;
    }
}
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

### Complexity Analysis

* **Time complexity:** $$O(N^2)$$.
* **Space complexity:** $$O(1)$$.

## Approach 2: Hash Table

### Intuition

This thought is similar to the \[\[[problem 1](../1-100/two-sum.md)\]\].

![](../../../.gitbook/assets/image%20%28184%29.png)

### Implementation

{% tabs %}
{% tab title="Java" %}
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> sums = new HashMap<Integer, Integer>();
        sums.put(0, 1);

        int count = 0;
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            count += sums.getOrDefault(sum - k, 0);
            sums.put(sum, sums.getOrDefault(sum, 0) + 1);
        }

        return count;
    }
}
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

### Complexity Analysis

* **Time complexity:** $$O(N)$$.
* **Space complexity:** $$O(N)$$.

