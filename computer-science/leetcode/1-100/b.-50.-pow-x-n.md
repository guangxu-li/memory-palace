---
description: Medium
---

# B 50. Pow\(x, n\)

## Description

Implement [pow\(_x_, _n_\)](http://www.cplusplus.com/reference/valarray/pow/), which calculates _x_ raised to the power _n_ \(xn\).

**Example 1:**

```text
Input: 2.00000, 10
Output: 1024.00000
```

**Example 2:**

```text
Input: 2.10000, 3
Output: 9.26100
```

**Example 3:**

```text
Input: 2.00000, -2
Output: 0.25000
Explanation: 2^-2 = 1/2^2 = 1/4 = 0.25
```

**Note:**

* -100.0 &lt; _x_ &lt; 100.0
* _n_ is a 32-bit signed integer, within the range $$[−2^{31}, 2^{31} − 1]$$

## Approach 1: Brute Force

```java
class Solution {
    public double myPow(double x, int n) {
        double ans = 1.0;
        double N = n;

        if (N < 0) {
            N = -N;
            x = 1 / x;
        }

        for (double i = 0.0; i < N; i++) {
            ans *= x;
        }

        return ans;
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(n)$$.
* **Space complexity:** $$O(1)$$.

## Approach 2: Fast Pow - Recursion

$$
(x^n)^2 = x^{2n}
$$

```java
class Solution {
    public double myPow(double x, int n) {
        return fastPow(x, n);
    }

    public double fastPow(double x, long N) {
        if (N < 0) {
            N = -N;
            x = 1 / x;
        } else if (N == 0) {
            return 1.0;
        }

        double ans = x;
        double y = 1;

        if (N % 2 == 1) {
            y = ans;
        }

        return y * fastPow(ans * ans, N / 2);
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(\log n)$$.
* **Space complexity:** $$O(\log {n})$$.

_Iterative_

```java
class Solution {
    public double myPow(double x, int n) {
        long N = n;

        if (N < 0) {
            N = -N;
            x = 1 / x;
        }

        double ans = 1;
        double currProduct = x;

        while (N > 0) {
            if (N % 2 == 1) {
                ans *= currProduct;
            }

            currProduct *= currProduct;

            N /= 2;
        }

        return ans;
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(\log n)$$.
* **Space complexity:** $$O(1)$$.

