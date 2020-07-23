---
description: Easy
---

# C 415. Add Strings

## [Description](https://leetcode.com/problems/add-strings/)

Given two non-negative integers `num1` and `num2` represented as string, return the sum of `num1` and `num2`.

**Note:**

1. The length of both `num1` and `num2` is &lt; 5100.
2. Both `num1` and `num2` contains only digits `0-9`.
3. Both `num1` and `num2` does not contain any leading zero.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

## Approach: Math

### Intuition

![](../../../.gitbook/assets/image%20%28191%29.png)

### Implementation

{% tabs %}
{% tab title="Java" %}
```java
class Solution {
    public String addStrings(String num1, String num2) {
        if (num1.length() == 0 || num1.charAt(0) == '0') {
            return num2;
        } else if (num2.length() == 0 || num2.charAt(0) == '0') {
            return num1;
        }

        char[] cs1 = num1.toCharArray();
        char[] cs2 = num2.toCharArray();

        char[] sum = new char[Math.max(cs1.length, cs2.length) + 1];

        for (int i = 1; i <= Math.max(cs1.length, cs2.length); i++) {
            int indice1 = cs1.length - i;
            int indice2 = cs2.length - i;
            int sumIndice = sum.length - i;

            int x = indice1 < 0 ? 0 : cs1[indice1] - '0';
            int y = indice2 < 0 ? 0 : cs2[indice2] - '0';
            int carry = sum[sumIndice] == 0 ? 0 : sum[sumIndice] - '0';

            int num = carry + x + y;
            sum[sumIndice] = (char) (num % 10 + (int) '0');
            sum[sumIndice - 1] = (char) (num / 10 + (int) '0');
        }

        if (sum[0] == '0') {
            return String.valueOf(sum, 1, sum.length - 1);
        } else {
            return String.valueOf(sum, 0, sum.length);
        }
    }
}
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

### Complexity Analysis

* **Time complexity:** $$O(\max(M, N)$$, where $$M$$ is the length of `num1` and $$N$$ is the length of `num2`.
* **Space complexity:** $$O(\max(M, N)$$.



