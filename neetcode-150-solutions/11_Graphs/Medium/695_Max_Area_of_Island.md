# <a href="https://leetcode.com/problems/max-area-of-island/" target="_blank">695. Max Area of Island</a>

### Problem Statement
You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island. Return the **maximum area** of an island in `grid`. If there is no island, return `0`.

### Approach
This problem is a direct extension of the "Number of Islands" problem. The strategy is the same: iterate through the grid and use a **Depth-First Search (DFS)** to find and sink each island. The only difference is that our DFS function now needs to calculate and return the area of the island it sinks.

1.  We have a main loop that scans every cell. We also maintain a `maxArea` variable.
2.  If a cell contains a `1`, we've found a new island. We then start a recursive DFS from that cell.
3.  This DFS function will do two things:
    -   It will "sink" the current piece of land by changing its value to `0` to mark it as visited.
    -   It will calculate the area. The area of the current piece of land is `1` plus the area of any connected land it finds in its four neighbors (which are calculated by the recursive calls).
4.  The DFS returns the total area of the island it just explored.
5.  Back in our main loop, we compare this returned area with our `maxArea` and update it if we've found a larger island.

### Implementation
```java
class Solution {
    private final int[][] DIRECTIONS = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    private int totalRows;
    private int totalColumns;

    public int maxAreaOfIsland(int[][] grid) {
        totalRows = grid.length;
        totalColumns = grid[0].length;
        int maxArea = 0;

        for (int row = 0 ; row < totalRows ; row++){
            for (int column = 0 ; column < totalColumns ; column++){
                if(grid[row][column] == 1){
                    maxArea = Math.max(maxArea, dfs(grid, row, column));
                }
            }
        }
        return maxArea;
    }

    public int dfs(int[][] grid, int row, int column){
        if(row < 0 || column < 0 || row >= totalRows || column >= totalColumns || grid[row][column] == 0){
            return 0;
        }

        grid[row][column] = 0;

        int area = 1;
        for(int[] direction : DIRECTIONS){
            area += dfs(grid, row + direction[0], column + direction[1]);
        }
        return area;
    }
}
``` 

### Complexity Analysis
Let `m` be the number of rows and `n` be the number of columns in the grid.

-   **Time Complexity:** O(m * n)
    -   The algorithm visits each cell in the grid at most a constant number of times.

-   **Space Complexity:** O(m * n)
    -   The space is determined by the maximum depth of the recursion stack. In the worst-case scenario (a grid filled entirely with land).
