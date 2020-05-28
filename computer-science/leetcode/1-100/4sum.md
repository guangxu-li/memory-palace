---
description: Medium
---

# B 18. 4Sum

## [Description](https://leetcode.com/problems/4sum/)

Given an array `nums` of _n_ integers and an integer `target`, are there elements _a_, _b_, _c_, and _d_ in `nums` such that _a_ + _b_ + _c_ + _d_ = `target`? Find all unique quadruplets in the array which gives the sum of `target`.

**Note:**

The solution set must not contain duplicate quadruplets.

**Example:**

```text
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

## Approach: N Times 3Sum

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);

        List<List<Integer>> ans = new ArrayList<>();

        if (nums.length < 4) {
            return ans;
        }

        for (int i = 0; i < nums.length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            List<List<Integer>> threeSum = threeSum(nums, i + 1, target - nums[i]);

            for (List<Integer> l : threeSum) {
                l.add(nums[i]);
                ans.add(l);
            }
        }

        return ans;
    }

    public List<List<Integer>> threeSum(int[] nums, int start, int target) {
        List<List<Integer>> ans = new ArrayList<>();

        for (int i = start; i < nums.length - 2; i++) {
            if (i > start && nums[i] == nums[i - 1]) {
                continue;
            }

            List<List<Integer>> twoSum = twoSum(nums, i + 1, target - nums[i]);

            for (List<Integer> l : twoSum) {
                l.add(nums[i]);
                ans.add(l);
            }
        }

        return ans;
    }

    public List<List<Integer>> twoSum(int nums[], int start, int target) {
        List<List<Integer>> ans = new ArrayList<>();

        int lo = start;
        int hi = nums.length - 1;

        while (lo < hi) {
            int sum = nums[lo] + nums[hi];

            if (sum < target || (lo > start && nums[lo] == nums[lo - 1])) {
                lo++;
            } else if (sum > target || (hi < nums.length - 1 && nums[hi] == nums[hi + 1])) {
                hi--;
            } else {
                ans.add(new ArrayList(Arrays.asList(nums[lo], nums[hi])));
                lo++;
                hi--;
            }
        }

        return ans;
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(n^3)$$. `twoSum() -> O(n), threeSum() -> n * O(n), fourSum() -> n * threeSum();`
* **Space complexity:** $$O(1)$$ if we exclude the recursion stack.

