---
description: Easy
---

# C 706. Design HashMap

## Description

Design a HashMap without using any built-in hash table libraries.

To be specific, your design should include these functions:

* `put(key, value)` : Insert a \(key, value\) pair into the HashMap. If the value already exists in the HashMap, update the value.
* `get(key)`: Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
* `remove(key)` : Remove the mapping for the value key if this map contains the mapping for the key.

  
**Example:**

```text
MyHashMap hashMap = new MyHashMap();
hashMap.put(1, 1);          
hashMap.put(2, 2);         
hashMap.get(1);            // returns 1
hashMap.get(3);            // returns -1 (not found)
hashMap.put(2, 1);          // update the existing value
hashMap.get(2);            // returns 1 
hashMap.remove(2);          // remove the mapping for 2
hashMap.get(2);            // returns -1 (not found) 
```

  
**Note:**

* All keys and values will be in the range of `[0, 1000000]`.
* The number of operations will be in the range of `[1, 10000]`.
* Please do not use the built-in HashMap library.

## Approach 1: Array

### Implementation

{% tabs %}
{% tab title="Java" %}
```java
class MyHashMap {
    private int[] map;

    /** Initialize your data structure here. */
    public MyHashMap() {
        map = new int[1000001];
    }

    /** value will always be non-negative. */
    public void put(int key, int value) {
        map[key] = value + 1;
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    public int get(int key) {
        return map[key] - 1;
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    public void remove(int key) {
        map[key] = 0;
    }
}
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

### Complexity Analysis

* **Time complexity:** $$O(1)$$.
* **Space complexity:** $$O(1)$$.

## Approach 2: Mod

### Intuition

As one of the most intuitive implementations, we could adopt the `modulo` operator as the hash function, since the key value is of integer type. In addition, in order to minimize the potential collisions, it is advisable to use a prime number as the base of modulo, e.g. 2069.

![](../../../.gitbook/assets/image%20%28202%29.png)

### Implementation

{% tabs %}
{% tab title="Java" %}
```java
class Pair<U, V> {
    public U key;
    public V value;

    public Pair(U key, V value) {
        this.key = key;
        this.value = value;
    }
}


class MyHashMap {
    List<List<Pair<Integer, Integer>>> keys;
    private int keySpace = 2729;

    /** Initialize your data structure here. */
    public MyHashMap() {
        keys = new ArrayList<>();

        for (int i = 0; i < keySpace; i++) {
            keys.add(new ArrayList<>());
        }
    }

    /** value will always be non-negative. */
    public void put(int key, int value) {
        List<Pair<Integer, Integer>> pairs = keys.get(key % keySpace);
        for (Pair<Integer, Integer> pair : pairs) {
            if (pair.key == key) {
                pair.value = value;
                return;
            }
        }

        pairs.add(new Pair<>(key, value));
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    public int get(int key) {
        for (Pair<Integer, Integer> pair : keys.get(key % keySpace)) {
            if (pair.key == key) {
                return pair.value;
            }
        }

        return -1;
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    public void remove(int key) {
        List<Pair<Integer, Integer>> pairs = keys.get(key % keySpace);
        for (Pair<Integer, Integer> pair : pairs) {
            if (pair.key == key) {
                pairs.remove(pair);
                return;
            }
        }
    }
}
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

### Complexity Analysis

* **Time complexity:** $$O(N/K)$$, where $$N$$ is the number of all possible keys and $$K$$ is the number of predefined buckets in the hashmap.
* **Space complexity:** $$O(K+M)$$ where $$M$$ is the number of unique keys that have been inserted into the hashmap.

