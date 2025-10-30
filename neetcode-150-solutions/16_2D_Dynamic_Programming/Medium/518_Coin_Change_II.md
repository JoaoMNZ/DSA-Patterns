# <a href="https://leetcode.com/problems/coin-change-2/" target="_blank">518. Coin Change II</a>

### Problem Statement
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

### Approach
We can solve this problem by building a 2D grid where we find the solution to smaller subproblems.

Let `cache[r][c]` be the number of ways to make amount `c` using the coins available from index `r` to the end of the `coins` array. Our goal is to find `cache[0][amount]`.

1.  **Base Case:** The number of ways to make an amount of `0` is always `1` (by choosing no coins). We fill the entire first column (`c = 0`) of our grid with `1`.
2.  **DP Formula:** For any other cell `cache[r][c]`, we have two choices:
    -   **Don't Use Coin `r`:** We skip this coin and move to the next row `r+1`. The number of ways is `cache[r+1][c]`.
    -   **Use Coin `r`:** We use the current coin. The number of ways is then the solution for the remaining amount, `c - coins[r]`. We stay on the same row `r` because we can use this coin multiple times. The number of ways is `cache[r][c - coins[r]]`.

The total number of ways for `cache[r][c]` is the sum of these two choices. By filling the grid column by column, from the bottom up, we ensure that the subproblems we need are already calculated when we get to them. The final answer will be in `cache[0][amount]`.

### Implementation
```java
class Solution {
    public int change(int amount, int[] coins) {
        int[][] cache = new int[coins.length][amount + 1];
        for(int c = 0 ; c <= amount ; c++)
            for(int r = coins.length - 1 ; r >= 0 ; r--){
                if(c == 0){
                    cache[r][c] = 1;
                    continue;
                }

                cache[r][c] = c - coins[r] >= 0 ? cache[r][c - coins[r]] : 0;
                cache[r][c] += r + 1 < coins.length ? cache[r+1][c] : 0;
            }
        
        return cache[0][amount];
    }
}
``` 

### Complexity Analysis
Let `a` be the target `amount` and `n` be the number of coin denominations.

-   **Time Complexity:** O(n * a)
    -   We must visit and perform a constant time calculation for each cell in our `n` by `a`.

-   **Space Complexity:** O(n * a)
    -   We create a 2D `cache` array of size `n * a` to store the results of all subproblems.
