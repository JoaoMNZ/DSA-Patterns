# <a href="https://leetcode.com/problems/coin-change/" target="_blank">322. Coin Change</a>

### Problem Statement
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

### Approach
We want to find the fewest coins needed to make up a specific `amount`.

We can define a subproblem: what is the fewest number of coins needed to make up a smaller `remaining_amount`?

The recursive logic is:
1.  The `dfs(remaining_amount)` function calculates the minimum coins needed.
2.  **Base Cases:**
    -   If `remaining_amount` is `0`, we've successfully made the amount with `0` additional coins. Return `0`.
    -   If `remaining_amount` is less than `0`, this path is invalid (we used a coin that was too large). Return a value indicating impossibility (like `Integer.MAX_VALUE`).
3.  **Memoization Check:** Before calculating, check if we've already computed the result for `remaining_amount`. If so, return the cached value.
4.  **Recursive Step:** If not cached, we try subtracting each `coin` from the `remaining_amount` and recursively call `dfs` for the new amount. For each valid result returned by the recursive calls, we add `1` (to count the coin we just used) and keep track of the minimum result found across all coin choices.
5.  Store the calculated minimum in the cache before returning it.

If the initial call returns `Integer.MAX_VALUE`, it means the amount cannot be made.

### Implementation
```java
import java.util.Arrays;

class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] cache = new int[amount + 1];
        Arrays.fill(cache, -1);

        int minCoins = dfs(coins, amount, cache);
        return minCoins != Integer.MAX_VALUE ? minCoins : -1;
    }

    public int dfs(int[] coins, int amount, int[] cache){
        if(amount == 0)
            return 0;

        if(amount < 0)
            return Integer.MAX_VALUE;

        if(cache[amount] != -1)
            return cache[amount];
        
        int fewest = Integer.MAX_VALUE;

        for(int coin : coins){
            int result = dfs(coins, amount - coin, cache);
            if(result != Integer.MAX_VALUE)
                result++;

            fewest = Math.min(fewest, result);
        }

        return cache[amount] = fewest;
    }
}
```

### Complexity Analysis
Let `t` be the target `amount` and `n` be the number of coin denominations.

-   **Time Complexity:** O(t * n)
    -   Due to memoization, the `dfs` function calculates the result for each amount from `0` to `t` only once. For each amount, we iterate through all `n` coins.

-   **Space Complexity:** O(t)
    -   The space is used by the `cache` array (size `t + 1`) and by the recursion stack, which can go up to `t` levels deep in the worst case.
