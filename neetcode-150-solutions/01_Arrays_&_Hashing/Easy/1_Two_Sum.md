# <a href="https://leetcode.com/problems/two-sum/" target="_blank">1. Two Sum</a>

### Problem Statement
Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`. You may assume that each input would have exactly one solution, and you may not use the same element twice. You can return the answer in any order.

### Approach
This problem can be solved efficiently using a `HashMap` to store values and their indices. The approach consists of two main passes over the data.

The logic is based on the mathematical equation `n2 = target - n1`. For any number `n1` in the array, we need to find its complement, `n2`.

1.  **First Pass:** Create a `HashMap` and iterate through the entire `nums` array, populating the map with each number as the key and its index as the value. If there are duplicate numbers, the map will store the index of the last occurrence.
2.  **Second Pass:** Iterate through the `nums` array again. For each number `n1` at index `i`:
    - Calculate the required complement: `complement = target - n1`.
    - Check if the `complement` exists as a key in the map.
    - Crucially, also check that the index of the complement stored in the map is not the same as the current index `i`. This prevents using the same element twice.
    - If both conditions are met, we have found the solution.

### Implementation
```java
import java.util.HashMap;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> indices = new HashMap<>();

        for(int i = 0 ; i < nums.length ; i++){
            indices.put(nums[i], i);
        }

        for(int i = 0 ; i < nums.length ; i++){
            int complement = target - nums[i];
            if(indices.containsKey(complement) && i != indices.get(complement)){
                return new int[] {i, indices.get(complement)};
            }
        }
        
        return new int[0];
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n)
  - We perform two separate passes through the array. The first loop to populate the map takes O(n) time. In the worst-case scenario the second loop to check for the complement also takes O(n) time.

- **Space Complexity:** O(n)
  - In the first pass, the HashMap stores all the elements of the input array.
