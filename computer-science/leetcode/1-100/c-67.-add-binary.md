---
description: Easy
---

# C 67. Add Binary

## [Description](https://leetcode.com/problems/add-binary/)

Given two binary strings, return their sum \(also a binary string\).

The input strings are both **non-empty** and contains only characters `1` or `0`.

**Example 1:**

```text
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```text
Input: a = "1010", b = "1011"
Output: "10101"
```

## Build-in Functions

```java
class Solution {
    public String addBinary(String a, String b) {
        return Integer.toBinaryString(Integer.parseInt(a, 2) + Integer.parseInt(b, 2));
    }
}
```

The overall algorithm has $$O(N + M)$$ time complexity and has a drawbakck:

* The limitation of length of input strings. Once the string is too long, the result of conversion into integers will not fit into `Integer, Long or BigInteger` .

## Approach 1: Bit-by-Bit computation

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder sb = new StringBuilder();
        int carry = 0;

        int i = a.length() - 1;
        int j = b.length() - 1;
        while (i >= 0 || j >= 0) {
            int x = i < 0 ? 0 : a.charAt(i) - '0';
            int y = j < 0 ? 0 : b.charAt(j) - '0';

            int sum = x + y + carry;
            carry = sum / 2;

            sb.append(sum % 2);

            i--;
            j--;
        }

        if (carry > 0) {
            sb.append(carry);
        }

        return sb.reverse().toString();
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(max(M, N))$$.
* **Space complexity:** $$O(max(M, N))$$

## Approach 2: Bit Manipulation

![](../../../.gitbook/assets/image%20%28109%29.png)

```java
class Solution {
    public String addBinary(String a, String b) {
        BigInteger x = new BigInteger(a, 2);
        BigInteger y = new BigInteger(b, 2);
        while (y.compareTo(new BigInteger("0")) > 0) {
            BigInteger sum = x.xor(y);
            y = x.and(y).shiftLeft(1);
            x = sum;
        }

        return x.toString(2);
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(M+N)$$.
* **Space complexity:** $$O(max(M, N))$$

