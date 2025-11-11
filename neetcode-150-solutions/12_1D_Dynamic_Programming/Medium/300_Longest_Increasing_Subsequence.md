# <a href="https://leetcode.com/problems/longest-increasing-subsequence/" target="_blank">300. Longest Increasing Subsequence</a>

### Problem Statement
Given an integer array `nums`, return the length of the longest **strictly increasing subsequence**.

### Approach
This problem can be solved efficiently using **Dynamic Programming**. We can use a `cache` array where `cache[i]` stores the length of the longest increasing subsequence (LIS) that *starts* at index `i`.

The logic is to fill this cache by iterating **backwards** from the end of the array.
1.  **Base Case:** We initialize every element in the `cache` to `1`, because any number by itself is a valid subsequence of length 1.
2.  **DP Logic:** When we are at index `i`, we look at all elements to its right (at index `j`).
    -   If `nums[i] < nums[j]`, it means we can create a *longer* subsequence by including `nums[i]` before the subsequence that starts at `j`.
    -   The new potential length would be `1 + cache[j]`.
    -   We update `cache[i]` to be the maximum of its current value and this new potential length.
3.  After the loops complete, the `cache` array will be filled with the lengths of the LIS starting at each position. The final answer is the maximum value in this `cache`.

### Implementation
```java
import java.util.Arrays;

class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] cache = new int[nums.length];
        Arrays.fill(cache, 1);

        for (int i = nums.length - 1; i >= 0; i--) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] < nums[j]) {
                    cache[i] = Math.max(cache[i], 1 + cache[j]);
                }
            }
        }

        int maxLIS = 0;
        for (int length : cache) {
            maxLIS = Math.max(maxLIS, length);
        }
        return maxLIS;
    }
}
``` 

### Complexity Analysis
Let `n` be the length of the input array `nums`.

-   **Time Complexity:** O(n²)
    -   We use two nested loops. The outer loop runs `n` times, and the inner loop runs, on average, `n/2` times, resulting in `O(n²)` comparisons.

-   **Space Complexity:** O(n)
    -   We create a `cache` array of size `n` to store the results of our subproblems.
