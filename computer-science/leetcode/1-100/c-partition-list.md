---
description: Easy
---

# C Partition List

## Description

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

