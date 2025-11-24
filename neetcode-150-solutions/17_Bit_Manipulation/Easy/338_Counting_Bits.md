# <a href="https://leetcode.com/problems/counting-bits/" target="_blank">338. Counting Bits</a>

### Problem Statement
Given an integer `n`, return an array `ans` of length `n + 1` such that for each `i` (`0 <= i <= n`), `ans[i]` is the **number of `1`'s** in the binary representation of `i`.

### Approach
This problem can be solved in linear time using **Dynamic Programming**. We can reuse the results from smaller numbers to compute the bit count for the current number.

The recurrence relation relies on bitwise operations:
1.  **Right Shift (`i >> 1`):** This operation removes the least significant bit (LSB). The resulting number `i >> 1` is strictly smaller than `i`, so we have already calculated its bit count in our DP array.
2.  **Check LSB (`i & 1`):** This checks if the bit we just removed was a `1` or a `0`.

Therefore, the number of 1s in `i` is equal to the number of 1s in `i >> 1` plus the LSB of `i`:
`dp[i] = dp[i >> 1] + (i & 1)`

Since we iterate from `1` to `n`, `dp[i >> 1]` is guaranteed to be available when calculating `dp[i]`.

### Implementation
```java
public class Solution {
    public int[] countBits(int n) {
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            dp[i] = dp[i >> 1] + (i & 1);
        }
        return dp;
    }
}
``` 

### Complexity Analysis
Let `n` be the input integer.

-   **Time Complexity:** O(n)
    -   We perform a single pass from `1` to `n`. Each iteration performs constant-time bitwise operations.

-   **Space Complexity:** O(1)
    -   Excluding the space required for the output array (which is `n + 1`), we use no additional auxiliary space.
