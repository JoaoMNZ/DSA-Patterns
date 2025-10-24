# <a href="https://leetcode.com/problems/house-robber-ii/" target="_blank">213. House Robber II</a>

### Problem Statement
You are a professional robber planning to rob houses along a street arranged in a **circle**. Each house has a certain amount of money stashed. The only constraint stopping you is that **adjacent houses** have security systems connected; it will automatically contact the police if two adjacent houses are broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight without alerting the police*.

### Approach
This problem is a variation of "House Robber I," with the added complication that the houses form a circle. This means the first and the last houses are considered adjacent.

This constraint leads to two main scenarios:
1.  The robber **robs the first house**. If he does this, he *cannot* rob the last house.
2.  The robber **does not rob the first house**. If he skips the first house, he is free to rob the last house.

We cannot calculate both cases simultaneously. Therefore, we solve the original "House Robber" problem twice:
-   **Case 1:** Find the maximum amount robbable from houses `0` to `n-2` (inclusive, ignoring the last house).
-   **Case 2:** Find the maximum amount robbable from houses `1` to `n-1` (inclusive, ignoring the first house).

The final answer is the maximum of the results from these two separate cases. The core logic for solving each subproblem remains the same as in "House Robber I," which can be done efficiently using dynamic programming. This implementation uses an iterative DP approach with constant space.

### Implementation
```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;

        if (n == 1){
            return nums[0];
        }

        return Math.max(helper(nums, 1, n), helper(nums, 0, n - 1));
    }

    public int helper(int[] nums, int begin, int end) {
        if (end - begin == 1) {
            return nums[begin];
        }

        int prev2 = nums[end - 1];
        int prev1 = Math.max(prev2, nums[end - 2]);

        for (int i = end - 3; i >= begin; i--) {
            int current = Math.max(prev1, nums[i] + prev2);
            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of houses (length of the `nums` array).

-   **Time Complexity:** O(n)
    -   The `helper` function iterates through a portion of the array once. We call this function twice, each on a subarray of size roughly `n`. Thus, the total time complexity is O(n) + O(n), which simplifies to O(n).

-   **Space Complexity:** O(1)
    -   The `helper` function uses only a few variables to store intermediate results, requiring constant extra space. The space used does not grow with the input size.
