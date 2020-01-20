---
description: Hard
---

# 4. Median of Two Sorted Arrays

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

## Solution

### Approach 1: Binary Search

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

1. $$i+j=m−i+n−j$$ If $$n \geq m$$, we just need to set: $$i = 0 \sim m, j = \frac{m + n + 1}{2} - i$$
2. $$B[j-1] \leq A[i]$$ and $$A[i-1] \leq B[j]$$

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
    public double findMedianSortedArrays(int[] A, int[] B) {
        int m = A.length;
        int n = B.length;
        if (m > n) { // to ensure m<=n, 0 <= j <= n
            int[] temp = A; A = B; B = temp;
            int tmp = m; m = n; n = tmp;
        }
        int iMin = 0, iMax = m, halfLen = (m + n + 1) / 2;
        while (iMin <= iMax) {
            int i = (iMin + iMax) / 2;
            int j = halfLen - i;
            if (i < iMax && B[j-1] > A[i]){
                iMin = i + 1; // i is too small
            }
            else if (i > iMin && A[i-1] > B[j]) {
                iMax = i - 1; // i is too big
            }
            else { // i is perfect
                int maxLeft = 0;
                if (i == 0) { maxLeft = B[j-1]; }
                else if (j == 0) { maxLeft = A[i-1]; }
                else { maxLeft = Math.max(A[i-1], B[j-1]); }
                if ( (m + n) % 2 == 1 ) { return maxLeft; }

                int minRight = 0;
                if (i == m) { minRight = B[j]; }
                else if (j == n) { minRight = A[i]; }
                else { minRight = Math.min(B[j], A[i]); }

                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(log(min(m, n)))$$. The search range is $$[0, min(m, n)]$$, After every loop, half of $$min(m, n)$$ will be reduced. So we only need $$log(min(m, n))$$loops.
* **Space complexity:** $$O(1)$$.

### Approach 2: 

#### Complexity Analysis

* **Time complexity:** 
* **Space complexity:** 

## Related Topics

[_Array_](https://leetcode.com/tag/Array/)_,_ [_Binary Search_](https://leetcode.com/tag/binary-search/)_,_ [_Divide and Conquer_](https://leetcode.com/tag/divide-and-conquer/)\_\_



