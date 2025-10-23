# <a href="https://leetcode.com/problems/house-robber/" target="_blank">198. House Robber</a>

### Problem Statement
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. The only constraint stopping you is that adjacent houses have security systems connected; **it will automatically contact the police if two adjacent houses are broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight without alerting the police*.

### Approach
This is a classic **Dynamic Programming** problem that can be solved using **Recursion with Memoization**. We want to find the maximum amount of money we can get starting from a given house `i`.

For any house `i`, we have two choices:
1.  **Rob house `i`:** If we rob this house, we gain `nums[i]` money, but we *cannot* rob the next house (`i+1`). We must skip to at least house `i+2`. The total money in this case is `nums[i] + dfs(nums, i + 2)`.
2.  **Skip house `i`:** If we skip this house, we gain `0` money from it, but we are free to consider robbing the next house (`i+1`). The total money in this case is `dfs(nums, i + 1)`.

Since we want the *maximum* amount, we choose the better of these two options.

To avoid recalculating the maximum amount for the same house multiple times, we use a `cache` array (memoization).

1.  The `dfs(i)` function calculates the maximum money we can rob starting from index `i`.
2.  **Base Case:** If `i` is out of bounds (`>= nums.length`), we can't rob any more houses, so the amount is `0`.
3.  **Memoization Check:** If `cache[i]` has already been computed, return the stored value.
4.  **Recursive Step:** Otherwise, calculate the result using the choice logic: `Math.max(nums[i] + dfs(nums, i + 2), dfs(nums, i + 1))`.
5.  Store the result in `cache[i]` before returning.

The initial call `dfs(nums, 0)` starts the process from the first house.

### Implementation
```java
import java.util.Arrays;

class Solution {
    int[] cache;
    
    public int rob(int[] nums) {
        cache = new int[nums.length];
        Arrays.fill(cache, -1);
        return dfs(nums, 0);
    }

    public int dfs(int[] nums, int i){
        if(i >= nums.length){
            return 0;
        }
        if(cache[i] != -1){
            return cache[i];
        }

        return cache[i] = Math.max(nums[i] + dfs(nums, i + 2), dfs(nums, i + 1));
    }
}
``` 

### Complexity Analysis
Let `n` be the number of houses (length of the `nums` array).

-   **Time Complexity:** O(n)
    -   Due to memoization, the recursive function `dfs` calculates the result for each house `i` only once.

-   **Space Complexity:** O(n)
    -   The space is used by the `cache` array (size `n`) and by the recursion stack, which can go up to `n` levels deep in the worst case.
