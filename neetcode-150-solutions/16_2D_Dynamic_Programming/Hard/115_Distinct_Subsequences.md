# <a href="https://leetcode.com/problems/distinct-subsequences/" target="_blank">115. Distinct Subsequences</a>

### Problem Statement
Given two strings `s` and `t`, return *the number of distinct subsequences of `s` which equals `t`*.

A string's **subsequence** is a new string formed from the original string by deleting some (can be none) of the characters without disturbing the remaining characters' relative positions.

### Approach
We create a `cache` grid where `cache[i][j]` represents the number of distinct subsequences of `s` (starting from index `i`) that can form `t` (starting from index `j`).

We fill this grid from the bottom-right corner upwards.
1.  **Base Case:** The last column (`j == t.length()`) is filled with `1`. This represents the end of the target string `t`. There is exactly one way to form an empty subsequence (by choosing no more characters).
2.  **DP Logic:** For any other cell `cache[i][j]`, we have two possibilities:
    -   **Skip `s[i]`:** The number of ways to form `t[j:]` is *at least* the number of ways to form it from `s[i+1:]`. So, we set `cache[i][j] = cache[i+1][j]`.
    -   **Match `s[i]`:** If `s.charAt(i) == t.charAt(j)`, it means we've found a match. We can *add* this to our count. The number of additional ways is the number of ways to form the rest of `t` (`t[j+1:]`) from the rest of `s` (`s[i+1:]`), which is `cache[i+1][j+1]`.
3.  **Final Answer:** The final result is found at `cache[0][0]`, which represents the number of ways the full string `s` can form the full string `t`.

### Implementation
```java
class Solution {
    public int numDistinct(String s, String t) {
        int n = s.length();
        int m = t.length();
        int[][] cache = new int[n + 1][m + 1];
        for(int row = 0 ; row <= n ; row++)
            cache[row][m] = 1;
        
        for(int row = n - 1 ; row >= 0 ; row--){
            for(int col = m - 1 ; col >= 0 ; col--){
                cache[row][col] = cache[row + 1][col];
                if(s.charAt(row) == t.charAt(col))
                    cache[row][col] += cache[row + 1][col + 1];
            }
        }
        
        return cache[0][0];
    }
}
``` 

### Complexity Analysis
Let `n` be the length of `s` and `m` be the length of `t`.

-   **Time Complexity:** O(n * m)
    -   We must visit each cell in our `n x m` grid.

-   **Space Complexity:** O(n * m)
    -   We create a 2D `cache` array of size `(n + 1) x (m + 1)`.
