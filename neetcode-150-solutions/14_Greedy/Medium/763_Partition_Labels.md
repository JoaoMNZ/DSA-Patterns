# <a href="https://leetcode.com/problems/partition-labels/" target="_blank">763. Partition Labels</a>

### Problem Statement
You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return a list of integers representing the size of these parts.

### Approach
This problem can be solved using a **Greedy** approach. The core idea is that a partition cannot end until we have seen the *last occurrence* of every character currently inside that partition.

1.  **First Pass (Map Last Indices):** We iterate through the string once to find the last index of every character. We store this in a `HashMap`
2.  **Second Pass (Create Partitions):** We iterate through the string again to determine the partitions.
    -   We maintain a variable `end` which represents the required end of the current partition.
    -   For each character we visit, we look up its last index. If its last index is greater than our current `end`, we extend the partition by updating `end = max(end, lastIndex)`.
    -   When our current iteration index `i` reaches `end`, it means we have visited all instances of all the characters in the current segment. We can close the partition, add its size to the result, and reset the size counter for the next partition.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;
import java.util.List;
import java.util.ArrayList;

class Solution {
    public List<Integer> partitionLabels(String s) {
        Map<Character, Integer> lastIndex = new HashMap<>();
        for(int i = 0 ; i < s.length() ; i++){
            lastIndex.put(s.charAt(i), i);
        }
        
        List<Integer> res = new ArrayList<>();
        int end = 0;
        int size = 0;
        for(int i = 0 ; i < s.length() ; i++){
            size++;
            end = Math.max(end, lastIndex.get(s.charAt(i)));
            if(i == end){
                res.add(size);
                size = 0;
            }
        }

        return res;
    }
}
``` 

### Complexity Analysis
Let `n` be the size of the string `s`.

-   **Time Complexity:** O(n)
    -   We iterate through the string twice: once to build the map and once to determine the partitions.

-   **Space Complexity:** O(1)
    -   We store the last index for the unique characters in `s`. Since `s` consists only of lowercase English letters, the map will contain at most 26 entries, which is constant space.
