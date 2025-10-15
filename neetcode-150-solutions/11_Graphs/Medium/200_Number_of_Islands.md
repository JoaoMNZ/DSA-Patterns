# <a href="https://leetcode.com/problems/number-of-islands/" target="_blank">200. Number of Islands</a>

### Problem Statement
Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### Approach
The core idea is to iterate through each cell of the grid. If we find a cell that is land (`'1'`), we know we've found a new island. We then need a way to find all the connected parts of this island and mark them as visited so we don't count the same island multiple times.

A **Depth-First Search (DFS)** is a perfect tool for this "sinking" process:
1.  We'll have a main loop that scans every cell of the grid.
2.  If a cell contains a `'1'`, it means we've discovered a new island. We increment our `islandCount`.
3.  We then immediately start a recursive DFS from that cell. This DFS will explore all adjacent land cells (up, down, left, right).
4.  To mark a cell as visited, we change its value from `'1'` to `'0'`. This "sinks" that piece of the island.
5.  The DFS continues until the entire connected landmass has been sunk (turned to `'0's).
6.  Our main loop then continues its scan, looking for the next `'1'` that hasn't been sunk yet.

### Implementation
```java
class Solution {
    private final int[][] DIRECTIONS = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    private int totalRows;
    private int totalColumns;

    public int numIslands(char[][] grid) {
        totalRows = grid.length;
        totalColumns = grid[0].length;
        int islandCount = 0;

        for (int row = 0; row < totalRows; row++) {
            for (int column = 0; column < totalColumns; column++) {
                if (grid[row][column] == '1') {
                    islandCount++;
                    dfs(grid, row, column);
                }
            }
        }

        return islandCount;
    }

    private void dfs(char[][] grid, int row, int column) {
        if (row < 0 || column < 0 || row >= totalRows || column >= totalColumns || grid[row][column] == '0') {
            return;
        }

        grid[row][column] = '0'; 

        for (int[] direction : DIRECTIONS) {
            dfs(grid, row + direction[0], column + direction[1]);
        }
    }
}
``` 

### Complexity Analysis
Let `m` be the number of rows and `n` be the number of columns in the grid.

-   **Time Complexity:** O(m * n)
    -   The algorithm visits each cell in the grid at most a constant number of times (once in the main loop and once during a DFS).

-   **Space Complexity:** O(m * n)
    -   The space is determined by the maximum depth of the recursion stack. In the worst-case scenario (a grid filled entirely with land), the recursion could go `m * n` levels deep.
