# <a href="https://leetcode.com/problems/unique-paths/" target="_blank">62. Unique Paths</a>

### Problem Statement
There is a robot on an `m x n` grid. The robot is initially located at the top-left corner (i.e., `grid[0][0]`). The robot tries to move to the bottom-right corner (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

### Approach
This is a classic **Dynamic Programming** problem. The main idea is to break the problem down into smaller, easier subproblems. The number of ways to reach any cell is simply the sum of the ways to reach the cell above it and the cell to its left.

We can use a 2D grid of the same `m x n` size to store the results of these subproblems. Each cell `grid[i][j]` will hold the number of unique paths from the start to that cell.

1.  **Base Cases:** For any cell in the first row or the first column, there is only **one** way to get there (by only moving right or only moving down). So, we can fill all these cells with the value `1`.

2.  **DP Formula:** For any other cell, the number of paths to reach it is the sum of the paths from the cell above (`grid[i-1][j]`) and the paths from the cell to the left (`grid[i][j-1]`).

By filling the grid this way, the final cell at `grid[m-1][n-1]` will contain the total number of unique paths to the destination.

### Implementation
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] grid = new int[m][n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 || j == 0) {
                    grid[i][j] = 1;
                } else {
                    grid[i][j] = grid[i - 1][j] + grid[i][j - 1];
                }
            }
        }

        return grid[m - 1][n - 1];
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(m * n)
  - We need to visit each cell in the `m x n` grid exactly once to calculate its value.

- **Space Complexity:** O(m * n)
  - We create a 2D grid of size `m x n` to store the results of our subproblems. The space used grows directly with the size of the input grid.
