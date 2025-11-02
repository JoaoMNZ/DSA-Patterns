# <a href="https://leetcode.com/problems/target-sum/" target="_blank">494. Target Sum</a>

### Problem Statement
You are given an integer array `nums` and an integer `target`. You want to build an **expression** out of `nums` by adding one of the symbols `+` or `-` before each integer in `nums` and then concatenating all the integers.

For example, if `nums = [2, 1]`, you can add a `+` before `2` and a `-` before `1` and concatenate them to build the expression `"+2-1"`.
Return *the number of different **expressions** that you can build which evaluates to `target`*.

### Approach
This iterative DP approach builds the solution bottom-up. We use an array of `HashMap`s, where `cache[i]` stores all possible sums that can be made using the first `i` numbers, and the *count* of how many ways there are to achieve each of those sums.

1.  **Base Case:** We initialize `cache[0].put(0, 1)`, which means there is exactly one way to make a sum of 0 with no numbers (the empty set).
2.  **Iteration:** We iterate through each number `num` in the `nums` array. For each `total` and `count` in the *previous* map (`cache[i]`), we update the *current* map (`cache[i+1]`) with the two possibilities:
    -   Add `num`: `cache[i+1].put(total + num, ...)`
    -   Subtract `num`: `cache[i+1].put(total - num, ...)`
    We add the `count` from the previous step to both new entries, accumulating the number of ways.
3.  **Result:** After iterating through all numbers, the final answer will be the count stored for `target` in the last map, `cache[n]`.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;

class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int n = nums.length;
        Map<Integer, Integer>[] cache = new HashMap[n + 1];
        for(int i = 0 ; i <= n ; i++){
            cache[i] = new HashMap<>();
        }
        cache[0].put(0, 1);

        for(int i = 0 ; i < n ; i++)
            for(Map.Entry<Integer, Integer> entry : cache[i].entrySet()){
                int total = entry.getKey();
                int count = entry.getValue();
                cache[i + 1].put(total + nums[i], cache[i + 1].getOrDefault(total + nums[i], 0) + count); 
                cache[i + 1].put(total - nums[i], cache[i + 1].getOrDefault(total - nums[i], 0) + count); 
            }
        
        return cache[n].getOrDefault(target, 0);
    } 
}
``` 

### Complexity Analysis
Let `n` be the length of the `nums` array and `m` be the total sum of all elements in `nums`.

-   **Time Complexity:** O(n * m)
    -   We have an outer loop that runs `n` times. In the worst case, the inner loop iterates over all possible sums, which can be proportional to the total sum `m`.

-   **Space Complexity:** O(n * m)
    -   We use an array of `n` HashMaps. In the worst case, each map can store `O(m)` entries.
