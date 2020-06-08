---
description: Hard
---

# A 4. Median of Two Sorted Arrays

## [Description](https://leetcode.com/problems/median-of-two-sorted-arrays/)

There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be $$O(log(m+n))$$.

You may assume **nums1** and **nums2** cannot be both empty.

**Example 1:**

```text
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```text
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

## Approach 1: Binary Search

```text
          left_part          |        right_part
    A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
    B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```

#### **Algorithm**

If we can ensure:

1. $$len(left\_part)=len(right\_part)$$
2. $$max(left\_part) \leq min(right\_part)$$

then we can know:

$$
median= \frac{max(left\_part)+min(right\_part)}{2}
​
$$

To ensure these two conditions, we need to ensure:

1. $$i+j=m−i+n−j$$ \(or: $$m - i + n - j + 1$$\) If $$n \geq m$$, we just need to set: $$i = 0 \sim m, j = \frac{m + n + 1}{2} - i$$
2. $$B[j-1] \leq A[i]$$ and $$A[i-1] \leq B[j]$$

{% hint style="info" %}
When $$i = 0$$and $$m > n$$, then for **nums2**, there's no enough elements to make the size of left part be equal to the one of right part.
{% endhint %}

**So the goal is to search** $$i \in [0, m]$$**such that** $$B[j-1] \leq A[i]$$ **and** $$A[i-1] \leq B[j]$$**, where** $$j=\frac{m+n+1}{2}-i$$**.**

#### **P**seudocode

```java
Initialize imin = 0, imax = m, i = (imin + imax) / 2, j = (m + n + 1) / 2 -i
Loop: if imin <= imax
    Initialize i = (imin + imax) / 2, j = (m + n + 1) / 2 - i
    if B[j - 1] > A[i]
        // increase i
        imin = i + 1;
    else if A[i - 1] > B[j]
        // decrease i
        imax = i - 1;
    else
        // perfect
        If length is odd, return Math.max(A[i - 1], B[j - 1])
        Else return (Math.max(A[i - 1], B[j - 1]) + Math.min(B[j], A[i])) / 2;
```

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int lo = 0;
        int hi = nums1.length;

        while (lo <= hi) {
            int i = lo + (hi - lo) / 2;
            int j = (nums1.length + nums2.length) / 2 - i;

            if (i > 0 && nums1[i - 1] > nums2[j]) {
                hi = i - 1;
            } else if (i < nums1.length && nums1[i] < nums2[j - 1]) {
                lo = i + 1;
            } else {

                double minRight = 0;

                if (i == nums1.length) {
                    minRight = nums2[j];
                } else if (j == nums2.length) {
                    minRight = nums1[i];
                } else {
                    minRight = Math.min(nums1[i], nums2[j]);
                }

                if (((nums1.length + nums2.length) % 2) != 0) {
                    return minRight;
                }

                double maxLeft = 0;
                if (i == 0) {
                    maxLeft = nums2[j - 1];
                } else if (j == 0) {
                    maxLeft = nums1[i - 1];
                } else {
                    maxLeft = Math.max(nums1[i - 1], nums2[j - 1]);
                }

                return (maxLeft + minRight) / 2;
            }
        }

        return 0.0;
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(log(min(m, n)))$$. The search range is $$[0, min(m, n)]$$, After every loop, half of $$min(m, n)$$ will be reduced. So we only need $$log(min(m, n))$$loops.
* **Space complexity:** $$O(1)$$.

