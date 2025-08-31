# <a href="https://leetcode.com/problems/longest-common-subsequence/" target="_blank">1143. Longest Common Subsequence</a>

### Problem Statement
Given two strings `text1` and `text2`, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters. A common subsequence of two strings is a subsequence that is common to both strings.

### Approach
This problem is a classic example of **2D Dynamic Programming**. We can build a grid (or DP table) to keep track of the length of the longest common subsequence as we compare the two strings character by character.

The grid will have a size of `(text2.length + 1) x (text1.length + 1)`. Each cell `grid[i][j]` will store the length of the longest common subsequence between the first `i` characters of `text2` and the first `j` characters of `text1`.

We fill the grid based on two simple rules when comparing the characters at the current position:
1.  **If the characters are the same:** We've found a character that extends our subsequence. The new length is 1 + the length of the subsequence from before these characters. We get this value from the top-left diagonal cell: `grid[i][j] = grid[i-1][j-1] + 1`.
2.  **If the characters are different:** The current characters don't match, so the subsequence doesn't get longer. We need to carry forward the longest subsequence found so far. We do this by taking the maximum value from either the cell to the left (`grid[i][j-1]`) or the cell above (`grid[i-1][j]`).

After filling the entire grid, the value in the bottom-right cell will be our final answer.

### Implementation
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n = text1.length();
        int m = text2.length();

        int[][] grid = new int[m + 1][n + 1];
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text2.charAt(i - 1) == text1.charAt(j - 1)) {
                    grid[i][j] = grid[i - 1][j - 1] + 1;
                } else {
                    grid[i][j] = Math.max(grid[i][j - 1], grid[i - 1][j]);
                }
            }
        }
        
        return grid[m][n];
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(m * n)
  - We need to visit each cell in the `m x n` grid exactly once to calculate its value.

- **Space Complexity:** O(m * n)
  - We create a 2D grid of size `m x n` to store the results of our subproblems. The space used grows directly with the size of the input strings.
