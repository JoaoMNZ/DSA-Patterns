# <a href="https://leetcode.com/problems/climbing-stairs/" target="_blank">70. Climbing Stairs</a>

### Problem Statement
You are climbing a staircase. It takes `n` steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

### Approach
This problem has overlapping subproblems, which makes it suitable for **Dynamic Programming**.

The number of ways to reach step `n` is the sum of:
-   The number of ways to reach step `n-1` (and take a 1-step jump).
-   The number of ways to reach step `n-2` (and take a 2-step jump).

This recursive relationship can be solved directly, but it would recalculate the same steps many times. To avoid this, we use memoization.

1.  We create a `cache` array to store the results for each step (`i`) once calculated. Initialize it with a value like -1 to indicate "not yet calculated."
2.  Our recursive function `dfs(i)` calculates the number of ways to reach the top (`n`) starting from step `i`.
3.  **Base Cases:**
    -   If `i` is equal to `n`, we've reached the top. There's only 1 way to do that (by arriving). Return `1`.
    -   If `i` is greater than `n`, we've overshot the top. This path is invalid. Return `0`.
4.  **Memoization Check:** Before calculating, check if `cache[i]` already has a valid result (`!= -1`). If yes, return the cached value directly.
5.  **Recursive Step:** If not cached, calculate the result by summing the results of the recursive calls for the next possible steps: `dfs(n, i + 1)` and `dfs(n, i + 2)`.
6.  Store this result in `cache[i]` before returning it.

The initial call `dfs(n, 0)` will start the process from the bottom (step 0).

### Implementation
```java
class Solution {
    int[] cache;

    public int climbStairs(int n) {
        cache = new int[n];
        for(int i = 0 ; i < n ; i++){
            cache[i] = -1;
        }
        
        return dfs(n, 0);
    }

    public int dfs(int n, int i){
        if(i >= n){
            return i == n ? 1 : 0;
        }
        if(cache[i] != -1){
            return cache[i];
        }
        return cache[i] = dfs(n, i + 1) + dfs(n, i + 2);
    }
}
``` 

### Complexity Analysis
Let `n` be the number of steps.

-   **Time Complexity:** O(n)
    -   Due to memoization, the recursive function `dfs` calculates the result for each step `i` (from 0 to `n`) only once.

-   **Space Complexity:** O(n)
    -   The space is used by the `cache` array, which stores results for `n+1` steps, and by the recursion stack, which can go up to `n` levels deep in the worst case.
