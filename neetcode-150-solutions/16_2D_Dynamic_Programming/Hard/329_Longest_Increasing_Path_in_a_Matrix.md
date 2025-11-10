# <a href="https://leetcode.com/problems/longest-increasing-path-in-a-matrix/" target="_blank">329. Longest Increasing Path in a Matrix</a>

### Problem Statement
Given an `m x n` integer matrix `matrix`, return the length of the longest increasing path in `matrix`.

From each cell, you can move in four directions: left, right, up, or down. You may not move diagonally or move outside the boundary.

### Approach
This problem can be solved using **Dynamic Programming** combined with a **Depth-First Search (DFS)**. A simple DFS would be too slow because it would re-calculate the longest path from the same cell multiple times.

To optimize, we use memoization (a `cache`). The `cache[r][c]` will store the length of the longest increasing path that *starts* at cell `(r, c)`.

1.  **Main Function:** We iterate through every cell in the grid, calling `dfs` on each one. We keep track of the maximum path length returned from any of these calls.
2.  **DFS Function:**
    -   **Base Cases:** The DFS stops and returns `0` if it goes out of bounds or if the next cell's value is not *greater* than the previous cell's value (as the path must be increasing).
    -   **Memoization Check:** If `cache[r][c]` is not `0`, it means we've already computed the result for this cell. We can return the cached value immediately.
    -   **Recursive Step:** If not cached, we explore all four neighbors. The longest path starting from the current cell is `1` (for the cell itself) plus the *maximum* of the paths returned from its valid neighbors.
    -   We store this result in `cache[r][c]` before returning it.

### Implementation
```java
class Solution {
    int[][] directions = new int[][]{ {1, 0}, {-1, 0}, {0, 1}, {0, -1} };
    int[][] cache;
    int m;
    int n;

    public int longestIncreasingPath(int[][] matrix) {
        m = matrix.length;
        n = matrix[0].length;
        cache = new int[m][n];

        int result = 0;
        for(int i = 0 ; i < m ; i++){
            for(int j = 0 ; j < n ; j++){
                result = Math.max(result, dfs(matrix, i, j, Integer.MIN_VALUE));
            }
        }
        
        return result;
    }

    public int dfs(int[][] matrix, int r, int c, int prev){
        if(r < 0 || r >= m || c < 0 || c >= n || matrix[r][c] <= prev){
            return 0;
        }
        if(cache[r][c] != 0){
            return cache[r][c];
        }

        int path = 1;
        for(int[] direction : directions){
            path = Math.max(path, 1 + dfs(matrix, r + direction[0], c + direction[1], matrix[r][c]));
        }

        return cache[r][c] = path;
    }
}
``` 

### Complexity Analysis
Let `m` be the number of rows and `n` be the number of columns.

-   **Time Complexity:** O(m * n)
    -   The algorithm computes the `dfs` result for each cell `(m * n)` exactly once.

-   **Space Complexity:** O(m * n)
    -   We use a `cache` array of size `m * n` to store the results. The recursion stack can also, in the worst case, go as deep as `O(m * n)`.
