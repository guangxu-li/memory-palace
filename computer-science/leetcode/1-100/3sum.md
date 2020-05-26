---
description: Medium
---

# B 15. 3Sum

## [Description](https://leetcode.com/problems/3sum/)

Given an array `nums` of _n_ integers, are there elements _a_, _b_, _c_ in `nums` such that _a_ + _b_ + _c_ = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```text
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

## Solution

### Approach 1: Two Pointers

We just need to modify [`twoSumII`](../101-200/two-sum-ii-input-array-is-sorted.md) to produce triplets and skip repeating values.

```text
3SUM(nums[])
    Sort nums[]
    For each element i in nums do
        if nums[i] > 0 then break;
        else if nums[i - 1] == nums[i] then continue;
        else TWOSUMII(nums[], i, res)

TWOSUMII(nums[], i, res)
    lo <- i + 1
    hi <- nums.length - 1
    
    while lo < hi, do
        sum <- nums[i] + nums[lo] + nums[hi]
        if sum < 0 || nums[lo - 1] == nums[lo]
            lo++
        else if sum > 0 || nums[hi] == nums[hi + 1]
            hi--
        else
            res <- [nums[i], nums[lo++], nums[hi--]
```

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();

        for (int i = 0; i < nums.length && nums[i] <= 0; i++) {
            if (i == 0 || nums[i - 1] != nums[i]) {
                twoSumII(nums, i, res);
            }
        }

        return res;
    }

    public void twoSumII(int[] nums, int i, List<List<Integer>> res) {
        int lo = i + 1;
        int hi = nums.length - 1;

        while (lo < hi) {
            int sum = nums[i] + nums[lo] + nums[hi];

            if (sum < 0 || (lo > i + 1 && nums[lo - 1] == nums[lo])) {
                lo++;
            } else if (sum > 0 || (hi < nums.length - 1) && nums[hi + 1] == nums[hi]) {
                hi--;
            } else {
                res.add(Arrays.asList(nums[i], nums[lo++], nums[hi--]));
            }
        }
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(n^2)$$. `twoSumII` is $$O(n)$$, and it runs $$n$$ times. Sorting the array takes $$O(n \log n)$$. In total, complexity is $$O(n^2)$$.
* **Space complexity:** From $$O(\log n)$$ to $$O(n)$$. It depends on the implementation of the sorting algorithm.

### Approach 2: Hash Set

We can put a combination of three values into a hash set to efficiently check whether we've found that triplet already. Values in a combination should be ordered \(e.g. ascending\). Otherwise, we can have results with the same values in the different positions.

Fortunately, we do not need to store all three values - storing the smallest and the largest ones is sufficient to identify any triplet. Because three values sum to the target, the third value will always be the same.

**Algorithm**

We process each value from left to right. For value `v`, we need to find all pairs whose sum is equal `-v`. To find such pairs, we apply the [Two Sum: One-pass Hash Table](two-sum.md#approach-3-one-pass-hash-table) approach to the rest of the array. To ensure unique triplets, we use a hash set `found` as described above.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Set<Pair> found = new HashSet<>();
        for (int i = 0; i < nums.length; ++i) {
            Set<Integer> seen = new HashSet<>();
            for (int j = i + 1; j < nums.length; ++j) {
                int complement = -nums[i] - nums[j];
                if (seen.contains(complement)) {
                    int v1 = Math.min(nums[i], Math.min(complement, nums[j]));
                    int v2 = Math.max(nums[i], Math.max(complement, nums[j]));
                    if (found.add(new Pair(v1, v2)))
                        res.add(Arrays.asList(nums[i], complement, nums[j]));
                }
                seen.add(nums[j]);
            }
        }
        return res;
    }
]
```

**Optimized Algorithm**

These optimizations don't change the big-O complexity, but help speed things up: 

1. Use another hash set `dups` to skip duplicates in the outer loop. 
2. Instead of re-populating a hash set every time in the inner loop, we can populate a hashmap once and then only modify values. After we process `nums[j]` in the inner loop, we set the hashmap value to `i`. This indicates that we can now use `nums[j]` to find pairs for `nums[i]`.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Set<Pair> found = new HashSet<>();
        Set<Integer> dups = new HashSet<>();
        Map<Integer, Integer> seen = new HashMap<>();
        for (int i = 0; i < nums.length; ++i)
            if (dups.add(nums[i]))
                for (int j = i + 1; j < nums.length; ++j) {
                    int complement = -nums[i] - nums[j];
                    if (seen.containsKey(complement) && seen.get(complement) == i) {
                        int v1 = Math.min(nums[i], Math.min(complement, nums[j]));
                        int v2 = Math.max(nums[i], Math.max(complement, nums[j]));
                        if (found.add(new Pair(v1, v2)))
                            res.add(Arrays.asList(nums[i], complement, nums[j]));
                    }
                    seen.put(nums[j], i);
                }
        return res;
        }
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(n^2)$$. We have outer and inner loops, each going through n elements. This algorithm is noticeably **slower** than the previous approach. Lookups in a hash set, though requiring a constant time, are expensive compared to the direct memory access.
* **Space complexity:** $$O(n^2)$$. We may need to store up to $$n^2$$ elements in a hash set for deduplication. We need the same amount of memory here as to store the output. In the worst case, there could be $$O(n^2)$$ triplets in the output, like for this example: `[-k, -k + 1, ..., -1, 0, 1, ... k - 1, k]`. Adding a new number to this sequence will produce `n / 3` new triplets.

## Related Topics

[_Array_](https://leetcode.com/tag/Array/)_,_ [_Two Pointers_](https://leetcode.com/tag/two-pointers/)\_\_

| Similar Questions | Difficulty |
| :--- | :--- |
| [Two Sum](two-sum.md) | Easy |
| [3Sum Closest](16.-3sum-closest.md) | _Medium_ |
| [4Sum](4sum.md) | _Medium_ |
| [3Sum Smaller](../201-300/259.-3sum-smaller.md) | _Medium_ |

