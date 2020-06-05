---
description: Easy
---

# C 170. Two Sum III - Data structure design

## [Description](https://leetcode.com/problems/two-sum-iii-data-structure-design/)

Design and implement a TwoSum class. It should support the following operations: `add` and `find`.

`add` - Add the number to an internal data structure.  
`find` - Find if there exists any pair of numbers which sum is equal to the value.

**Example 1:**

```text
add(1); add(3); add(5);
find(4) -> true
find(7) -> false
```

**Example 2:**

```text
add(3); add(1); add(2);
find(3) -> true
find(6) -> false
```

## Approach 1: Sorted List

**Intuition & Algorithm**

At the beginning of the find function, we sort the list if it's not sorted.

```java
class TwoSum {
    private List<Integer> nums;

    private boolean isSorted;

    /** Initialize your data structure here. */
    public TwoSum() {
        nums = new ArrayList<>();

        isSorted = true;
    }

    /** Add the number to an internal data structure.. */
    public void add(int number) {
        nums.add(number);
        isSorted = false;
    }

    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {
        if (!isSorted) {
            Collections.sort(nums);
            isSorted = true;
        }

        int lo = 0;
        int hi = nums.size() - 1;

        while (lo < hi) {
            int sum = nums.get(lo) + nums.get(hi);

            if (sum == value) {
                return true;
            } else if (sum > value) {
                hi--;
            } else {
                lo++;
            }
        }

        return false;
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$n\log{n}$$.
* **Space complexity:** $$O(n)$$. We need $$O(n)$$ space to store numbers.

## Approach 2: Hash Table

**Intuition & Algorithm**

```java
class TwoSum {
    private Map<Integer, Integer> nums;

    /** Initialize your data structure here. */
    public TwoSum() {
        nums = new HashMap<>();
    }

    /** Add the number to an internal data structure.. */
    public void add(int number) {
        nums.put(number, nums.getOrDefault(number, 0) + 1);
    }

    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {
        for (Map.Entry<Integer, Integer> entry : nums.entrySet()) {
            int complement = value - entry.getKey();

            if (nums.containsKey(complement)) {
                if (complement != entry.getKey() || nums.get(complement) >= 2) {
                    return true;
                }
            }
        }

        return false;
    }
}
```

**Complexity Analysis**

* **Time complexity:** $$O(n)$$.
* **Space complexity:** $$O(n)$$.

