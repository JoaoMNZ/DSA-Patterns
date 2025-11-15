# <a href="https://leetcode.com/problems/partition-equal-subset-sum/" target="_blank">416. Partition Equal Subset Sum</a>

### Problem Statement
Given an integer array `nums`, return `true` if you can partition the array into two subsets such that the sum of the elements in both subsets is equal, or `false` otherwise.

### Approach
This problem is a subset sum problem, which can be solved with **Dynamic Programming**. The key insight is that if the array can be partitioned into two equal subsets, each subset must sum to `totalSum / 2`.

1.  **Initial Check:** First, we calculate the `totalSum` of the array. If this sum is odd, it's impossible to split it into two equal integer halves, so we can immediately return `false`.
2.  **Target Sum:** Our goal is to find if there is *any* subset of `nums` that adds up to `target = totalSum / 2`.
3.  **DP State:** We use a 2D `cache` where `cache[i][j]` answers the question: "Is it possible to reach the `target` sum, given we are at index `i` of the `nums` array and our *current sum* is `j`?"
4.  **Base Case:** We set the entire column for the `target` sum to `true` (`cache[i][target] = true`). This means if our current sum ever reaches the target, we have successfully found a valid partition.
5.  **DP Logic:** We fill the grid from the bottom-right. For any state `cache[i][j]`, we have two choices:
    -   **Skip `nums[i]`:** The result is whatever the answer was for the next item: `cache[i+1][j]`.
    -   **Use `nums[i]`:** If using the number doesn't exceed the target (`j + nums[i] <= target`), we check the result of that state: `cache[i+1][j + nums[i]]`.
    If either of these paths is `true`, then `cache[i][j]` becomes `true`.
6.  **Final Answer:** The solution is `cache[0][0]`, which asks: "Is it possible to reach the target sum, starting at index 0 with a current sum of 0?"

### Implementation
```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int num : nums)
            sum += num;
        
        if(sum % 2 != 0)
            return false;
        
        int target = sum / 2;
        boolean[][] cache = new boolean[nums.length + 1][target + 1];
        for(int i = 0 ; i < nums.length ; i++)
            cache[i][target] = true;

        for(int col = target - 1 ; col >= 0 ; col--){
            for(int row = nums.length - 1 ; row >= 0 ; row--){
                if(col + nums[row] <= target)
                    cache[row][col] = cache[row + 1][col + nums[row]];

                cache[row][col] = cache[row][col] || cache[row + 1][col];
            }
        }

        return cache[0][0];
    }
}
``` 

### Complexity Analysis
Let `n` be the length of the `nums` array and `t` be the target sum (`totalSum / 2`).

-   **Time Complexity:** O(n * t)
    -   We must visit and perform a constant-time calculation for each cell in our `(n+1) x (t+1)` grid.

-   **Space Complexity:** O(n * t)
    -   We create a 2D `cache` array of size `(n+1) x (t+1)` to store the results of all subproblems.
