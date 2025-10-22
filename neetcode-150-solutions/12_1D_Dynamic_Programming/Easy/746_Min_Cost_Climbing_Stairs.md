# <a href="https://leetcode.com/problems/min-cost-climbing-stairs/" target="_blank">746. Min Cost Climbing Stairs</a>

### Problem Statement
You are given an integer array `cost` where `cost[i]` is the cost of the `i`th step on a staircase. Once you pay the cost, you can either climb one or two steps. You can either start from the step with index `0`, or the step with index `1`.

Return the *minimum cost* to reach the top of the floor (which is considered one step past the last index).

### Approach
This problem asks for the minimum cost, which suggests a **Dynamic Programming** algorithm.

From step `i`, what's the minimum cost to reach the *top*? It's the cost of the current step (`cost[i]`) plus the minimum cost to reach the top from either step `i+1` or step `i+2`.

This gives us a recursive relationship. To avoid recalculating the minimum cost for the same step multiple times, we use memoization.

1.  We create a `cache` array to store the minimum cost to reach the top starting from step `i`.
2.  Our recursive function `dfs(i)` calculates this minimum cost.
3.  **Base Case:** If `i` is beyond the end of the stairs (`>= cost.length`), the cost from here is `0` (we've already reached the top).
4.  **Memoization Check:** If `cache[i]` has already been calculated, return the stored value.
5.  **Recursive Step:** Otherwise, calculate the cost: `cost[i] + Math.min(dfs(cost, i + 1), dfs(cost, i + 2))`.
6.  Store this result in `cache[i]` before returning it.

Since we can start at either index 0 or index 1, the final answer is the minimum of `dfs(cost, 0)` and `dfs(cost, 1)`.

### Implementation
```java
class Solution {
    int[] cache;
    
    public int minCostClimbingStairs(int[] cost) {
        cache = new int[cost.length];
        for(int i = 0 ; i < cost.length ; i++){
            cache[i] = -1;
        }

        return Math.min(dfs(cost, 0), dfs(cost, 1));
    }

    int dfs(int[] cost, int i){
        if(i >= cost.length){
            return 0;
        }

        if(cache[i] != -1){
            return cache[i];
        }

        return cache[i] = cost[i] + Math.min(dfs(cost, i + 1), dfs(cost, i + 2));
    }
}
``` 

### Complexity Analysis
Let `n` be the number of steps (length of the `cost` array).

-   **Time Complexity:** O(n)
    -   Due to memoization, the recursive function `dfs` calculates the result for each step `i` only once.

-   **Space Complexity:** O(n)
    -   The space is used by the `cache` array (size `n`) and by the recursion stack, which can go up to `n` levels deep in the worst case.
