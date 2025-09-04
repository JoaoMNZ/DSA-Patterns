# <a href="https://leetcode.com/problems/time-based-key-value-store/" target="_blank">981. Time Based Key-Value Store</a>

### Problem Statement
Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

Implement the `TimeMap` class:
- `TimeMap()` Initializes the object of the data structure.
- `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value` at the given time `timestamp`.
- `String get(String key, int timestamp)` Returns a value such that `set` was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.

### Approach
The data structure needs to handle a single key with multiple values. A `HashMap` is a perfect choice, where the key is a `String` and the value is a `List` of pairs. Each pair will store a `value` and its corresponding `timestamp`.

**For the `set` method:**
The problem states that timestamps are strictly increasing. This means that when we add a new pair to a key's list, it will always be sorted by timestamp. This is the key property that allows for an efficient `get` method.

**For the `get` method:**
Since the list of pairs for any key is sorted by timestamp, we can use **Binary Search** to find the correct value quickly. The search is slightly modified because we need to find the value whose timestamp is the largest one that is still less than or equal to the target `timestamp`.

The logic is:
1.  We perform a binary search on the list of pairs.
2.  We keep a `result` variable to store the best valid value found so far.
3.  If the middle element's timestamp is less than or equal to our target `timestamp`, it's a valid answer. We save its value in `result` and then search in the **right half** to see if a better answer (a more recent valid timestamp) exists.
4.  If the middle element's timestamp is greater than our target, it's too recent, so we must search in the **left half**.
5.  After the loop, `result` will hold the correct answer.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;
import java.util.List;
import java.util.ArrayList;

class TimeMap {
    private final Map<String, List<Pair>> map;

    // A private class to hold the value and timestamp together
    private static class Pair {
        public final String value;
        public final int timestamp;
        
        Pair(String value, int timestamp){
            this.value = value;
            this.timestamp = timestamp;
        }
    }

    public TimeMap() {
        map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        map.computeIfAbsent(key, k -> new ArrayList<>())
           .add(new Pair(value, timestamp));
    }
    
    public String get(String key, int timestamp) {
        List<Pair> list = map.get(key);
        if (list == null || list.isEmpty()) {
            return "";
        }

        int left = 0;
        int right = list.size() - 1;
        String result = "";
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            Pair midPair = list.get(mid);
            
            if (midPair.timestamp <= timestamp) {
                // This is a possible answer, save it and check for a better one
                result = midPair.value;
                left = mid + 1;
            } else {
                // This timestamp is too high, look for an earlier one
                right = mid - 1;
            }
        }
        return result;
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:**
    -   `set`: **O(1)** on average, as adding to a HashMap and an ArrayList are constant time operations.
    -   `get`: **O(log n)**, where `n` is the number of entries for a specific key, due to the Binary Search.

-   **Space Complexity:** O(m * n)
    -   Let `m` be the number of unique keys and `n` be the average number of values per key. The space is required to store all the key and value-timestamp pairs.
