---
description: Medium
---

# B 43. Multiply Strings

## [Description](https://leetcode.com/problems/multiply-strings/)

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Example 1:**

```text
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```text
Input: num1 = "123", num2 = "456"
Output: "56088"
```

**Note:**

1. The length of both `num1` and `num2` is &lt; 110.
2. Both `num1` and `num2` contain only digits `0-9`.
3. Both `num1` and `num2` do not contain any leading zero, except the number 0 itself.
4. You **must not use any built-in BigInteger library** or **convert the inputs to integer** directly.

## Approach: Multiplication

![](../../../.gitbook/assets/image%20%2874%29.png)

```java
class Solution {
    public String multiply(String num1, String num2) {
        int[] result = new int[num1.length() + num2.length()];

        for (int i = num1.length() - 1; i >= 0; i--) {
            for (int j = num2.length() - 1; j >= 0; j--) {
                int curr = i + j + 1;
                int next = i + j;

                int num = (num1.charAt(i) - '0') * (num2.charAt(j) - '0') + result[curr];

                result[curr] = num % 10;
                result[next] += num / 10;
            }
        }

        StringBuilder ans = new StringBuilder();
        for (int i : result) {
            if (ans.length() == 0 && i == 0) {
                continue;
            } else {
                ans.append(i);
            }
        }

        return ans.length() == 0 ? "0" : ans.toString();
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(M\cdot N)$$, where $$M$$ is the size of `num1` , $$N$$ is the size of `num2` .
* **Space complexity:** $$O(M+N)$$.

