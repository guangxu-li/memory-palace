---
description: 'https://leetcode.com/problems/add-two-numbers/'
---

# 2. Add Two Numbers

## Description

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```text
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

## Solution

### Elementary Math

#### Intuition

![Figure 1. Visualization of the addition of two numbers: 342 + 465 = 807342+465=807.](../../.gitbook/assets/2_add_two_numbers.svg)

#### Algorithm

Just like usually adding two numbers, we begin from least-significant digits. If the sum of two numbers is larger than 10, then we need to add the carry to the next digit.

**Pseudocode**

```text
1. Initialize a node to containing results.
2. Loop through inputs.
    1. set sum = x + y + carry
    2. set carry = sum / 10
    3. node.next = ListNode(sum % 10)
    4. next one input listnode.
3. If carry = 1, append a new node with digit 1.
```

{% hint style="warning" %}
Step 3 is necessary. Without it, the program can't work for 456 + 654.
{% endhint %}

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}
```

#### Complexity Analysis

* **Time complexity:** $$O(max(m,n))$$. m and n represents the length of l1 and l2 respectively.
* **Space complexity:** $$O(max(m,n))$$. The length of the new list is at most $$max(m,n)+1$$.

