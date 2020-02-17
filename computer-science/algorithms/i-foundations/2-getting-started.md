# 2 Getting Started

## 2.1 Insertion sort

![Figure 2.2 The operation of INSERTION-SORT on the array A = &amp;lt; 5, 2, 4, 6, 1, 3&amp;gt;.](../../../.gitbook/assets/algorithms-figure-2.2.jpg)

```text
INSERTION-SORT
for j = 2 to A.length
    key = A[j]
    //Insert A[j] into the sorted sequence A[1 .. j - 1].
    i = j - 1
    while i > 0 and A[i] > key
        A[i + 1] = A[i]
        i = i - 1
    A[i + 1] = key
```

### Loop invariants and the correctness of insertion sort

{% hint style="info" %}
**Loop Invariant:**

At the start of each iteration of the **for** loop of lines 1-8, the subarray A\[1 .. j - 1\] consists of the elements of originally in A\[1 .. j - 1\], but in sorted order.
{% endhint %}

Three things about a loop invariant:

* **Initialization:** It is true prior to the first iteration of the loop.
* **Maintenance:** If it is true before an iteration of the loop, it remains true before the next iteration.
* **Termination:** When the loop terminates, the invariant gives us a useful property that helps show that the algorithm is correct.

## 2.2 Analyzing algorithms



