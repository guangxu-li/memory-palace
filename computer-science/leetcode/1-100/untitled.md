---
description: Medium
---

# B 29. Divide Two Integers

## Description

Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero, which means losing its fractional part. For example, `truncate(8.345) = 8` and `truncate(-2.7335) = -2`.

**Example 1:**

```text
Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = truncate(3.33333..) = 3.
```

**Example 2:**

```text
Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = truncate(-2.33333..) = -2.
```

**Note:**

* Both dividend and divisor will be 32-bit signed integers.
* The divisor will never be 0.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $$[−2^{31},  2^{31} − 1]$$. For the purpose of this problem, assume that your function **returns 231 − 1 when the division result overflows**.

## Approach 1: Repeated Subtraction

**Intution & Algorithm**

The simplest way is to subtract `5` from `15` repeatedly until we can no longer do so.

```text
15 - 5 = 10
10 - 5 = 5
 5 - 5  = 0
```

Because we were able to do `3` subtractions, we know the answer to `15 / 5` is `3`.

```java
class Solution {
    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }

        int count = 0;
        int op = -1;

        if (dividend > 0) {
            dividend = -dividend;
        } else {
            op = -op;
        }

        if (divisor > 0) {
            divisor = -divisor;
        } else {
            op = -op;
        }

        while (dividend <= divisor) {
            dividend -= divisor;

            count--;
        }

        return op == -1 ? -count : count;
    }
}
```

{% hint style="info" %}
**Consider the case of `Integer.MIN_VALUE / -1`** ``
{% endhint %}

#### Complexity Analysis

* **Time complexity:** $$O(n)$$. Consider the worst case wehre the divisor is $$1$$.
* **Space complexity:** $$O(1)$$.

## Approach 2: Repeated Exponential Searches

**Intution & Algorithm**

Let's say we have a dividend of `93706` and a divisor of `157` . We repeatedly double `157` until it's bigger than `93706` .

> `157, 314, 628, 1256, 2512, 5024, 10048, 20096, 40192, 80384, 160768 <- Too big`

Until now, in the dividend of `93706` , there are $$2^9$$ copies of `157` . And the leftover is `13322` . We could  repeat the same process for dividend of `13322` and the divisor of `157` . When a new dividend can't fit any divisor, then it's the end.

```java
class Solution {
    private static int HALF_INT_MIN = -1073741824;

    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }

        int quotient = 0;
        int op = -1;

        if (dividend > 0) {
            dividend = -dividend;
        } else {
            op = -op;
        }

        if (divisor > 0) {
            divisor = -divisor;
        } else {
            op = -op;
        }

        while (dividend <= divisor) {
            int pow = -1;
            int value = divisor;

            while (value >= HALF_INT_MIN && value + value >= dividend) {
                pow += pow;
                value += value;
            }

            quotient += pow;
            dividend -= value;
        }

        return op == -1 ? -quotient : quotient;
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(\log^2{n})$$. We started by performing an exponential search too find the biggest number that fits into the current dividend, which takes $$O(\log{n})$$. With the dividend at least halving after updating dividend, there couldn't have been more than $$\log{n}$$ times.
* **Space complexity:** $$O(1)$$.

## Approach 3: Adding Powers of Two

**Intution & Algorithm**

In the previous approach, each time we do a search, we repeatedly go through the same doubles to find the largest.

Instead of doing this, we should find a way so that we can compute the sequence just once and then use the results from this to compute our quotient.

{% hint style="info" %}
The difference will always be less than the previous doubling of the divisor that fits into it.
{% endhint %}

![](../../../.gitbook/assets/image%20%2839%29.png)

```java
class Solution {
    private static int HALF_INT_MIN = -1073741824;

    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }

        int op = -1;

        if (dividend > 0) {
            dividend = -dividend;
        } else {
            op = -op;
        }

        if (divisor > 0) {
            divisor = -divisor;
        } else {
            op = -op;
        }

        List<Integer> doubles = new ArrayList<>();
        List<Integer> pows = new ArrayList<>();

        int pow = -1;
        pows.add(pow);

        int value = divisor;
        doubles.add(divisor);

        while (value >= HALF_INT_MIN && value + value >= dividend) {
            value += value;
            doubles.add(value);

            pow += pow;
            pows.add(pow);
        }

        int quotient = 0;

        for (int i = doubles.size() - 1; i >= 0; i--) {
            if (dividend <= doubles.get(i)) {
                quotient += pows.get(i);
                dividend -= doubles.get(i);
            }
        }

        return op == -1 ? -quotient : quotient;
    }
}
```

#### Complexity Analysis

* **Time complexity:** $$O(\log{n})$$. It takes $$O(\log{n})$$ to search the largest double that fits into dividend. After this, since the doubles list has been created it only takes $$O(\log {n})$$ to search the largest double for the following dividend. In total, it takes $$O(2 * \log{n})$$ .
* **Space complexity:** $$O(\log{n})$$.



## Approach 4: Adding Powers of Two with Bit-Shifting

**Intution & Algorithm**

In the previous approach, we maintain a list to store doubles. Instead of creating a list, we could only store the largest doubles found at the beginning. For the following search, we could divide it by 2 repeatly until we found it's less than the dividend.

Even though it's not allowed to use division according to the decription, we can still achieve this by using bit shift.

{% hint style="warning" %}
One potential pitfall with the right-shift operator is using it on negative _odd_ numbers.

```java
int a = -1020;
a = a >> 1;
System.out.println(a);
// Prints -510. Great!
int b = -1021;
b = b >> 1;
System.out.println(b);
// Prints -511. Ugghh.
```

The solution is to add 1 before doing the bit-shift on a _negative_ number.

```java
int a = -1020;
a = (a + 1) >> 1;
System.out.println(a);
// Prints -510. Great!
int b = -1021;
b = (b + 1) >> 1;
System.out.println(b);
// Prints -510. Yay!
```
{% endhint %}

```java
class Solution {
    private static int HALF_INT_MIN = -1073741824;

    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }

        int op = -1;

        if (dividend > 0) {
            dividend = -dividend;
        } else {
            op = -op;
        }

        if (divisor > 0) {
            divisor = -divisor;
        } else {
            op = -op;
        }

        int pow = -1;
        int value = divisor;

        while (value >= HALF_INT_MIN && value + value >= dividend) {
            value += value;
            pow += pow;
        }

        int quotient = 0;

        while (dividend <= divisor) {
            if (dividend <= value) {
                dividend -= value;
                quotient += pow;
            }

            value >>= 1;
            pow >>= 1;
        }

        return op == -1 ? -quotient : quotient;
    }
}
```

#### Complexity Analysis

* **Time complexity:** 
* **Space complexity:** 

## Approach 5: 

**Intution & Algorithm**

```java

```

#### Complexity Analysis

* **Time complexity:** 
* **Space complexity:** 



