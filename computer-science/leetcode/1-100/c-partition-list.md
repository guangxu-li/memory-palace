---
description: Easy
---

# C 86. Partition List

## Description

Given a linked list and a value _x_, partition it such that all nodes less than _x_ come before nodes greater than or equal to _x_.

You should preserve the original relative order of the nodes in each of the two partitions.

**Example:**

```text
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5
```

## Approach: Two Pointers

![](../../../.gitbook/assets/image%20%28127%29.png)

![](../../../.gitbook/assets/image%20%28129%29.png)

![](../../../.gitbook/assets/image%20%28130%29.png)

![](../../../.gitbook/assets/image%20%28128%29.png)

```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        if (head == null || head.next == null) {
            return head;
        }

        ListNode left = new ListNode(0);
        ListNode right = new ListNode(0);
        ListNode leftHead = left;
        ListNode rightHead = right;

        while (head != null) {
            if (head.val < x) {
                left.next = head;
                left = left.next;
            } else {
                right.next = head;
                right = right.next;
            }

            head = head.next;
        }

        left.next = rightHead.next;
        right.next = null;

        return leftHead.next;
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(N)$$.
* **Space complexity:** $$O(1)$$.

