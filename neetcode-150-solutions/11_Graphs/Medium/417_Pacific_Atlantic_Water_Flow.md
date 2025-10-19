# <a href="https://leetcode.com/problems/pacific-atlantic-water-flow/" target="_blank">417. Pacific Atlantic Water Flow</a>

### Problem Statement
There is an `m x n` rectangular island that borders both the **Pacific Ocean** and the **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the height above sea level of the cell at coordinate `(r, c)`.

Water can flow from any cell adjacent to an ocean into the ocean. Water can only flow from a cell to an adjacent cell (up, down, left, right) if the adjacent cell's height is **less than or equal to** the current cell's height.

Return *a 2D list of grid coordinates `result[i] = [ri, ci]` where water can flow from `(ri, ci)` to **both** the Pacific and Atlantic oceans*.

### Approach
Instead of trying to figure out where water can flow *from* each cell *to* the oceans, it's much easier to think about where water can flow *from* the oceans *inwards* to the cells. We are looking for cells that can be reached by water flowing from both the Pacific and the Atlantic.

This can be solved efficiently using two **Depth-First Searches (DFS)** (or BFS):

1.  **Mark Reachable Cells:**
    -   We'll use two boolean grids, `pacific` and `atlantic`, of the same size as the island, to mark cells reachable from each ocean.
    -   Start a DFS from all cells directly bordering the Pacific (top row and left column). This DFS will mark all cells in the `pacific` grid that can be reached by water flowing *backwards* (from lower or equal height to higher height) from the Pacific.
    -   Do the same for the Atlantic ocean, starting DFS from the bottom row and right column, marking reachable cells in the `atlantic` grid.

2.  **Find the Intersection:**
    -   After running both sets of DFS, we iterate through every cell `(r, c)` on the island.
    -   If a cell is marked `true` in **both** the `pacific` and `atlantic` grids, it means water can flow from this cell to both oceans. We add its coordinates to our final result list.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;
import java.util.Arrays;

class Solution {
    int[][] DIRECTIONS = new int[][]{{0,1}, {0,-1}, {1,0}, {-1,0}};
    int rows;
    int columns;

    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        rows = heights.length;
        columns = heights[0].length;
        boolean[][] pacific = new boolean[rows][columns];
        boolean[][] atlantic = new boolean[rows][columns];

        for(int c = 0 ; c < columns ; c++){
            dfs(heights, pacific, 0, c);
            dfs(heights, atlantic, rows - 1, c);
        }
        for(int r = 0 ; r < rows ; r++){
            dfs(heights, pacific, r, 0);
            dfs(heights, atlantic, r, columns - 1);
        }

        List<List<Integer>> result = new ArrayList<>();
        for(int r = 0 ; r < rows ; r++){
            for(int c = 0 ; c < columns ; c++){
                if(pacific[r][c] && atlantic[r][c]){
                    result.add(Arrays.asList(r, c));
                }
            }
        }
        return result;
    }

    public void dfs(int[][] heights, boolean[][] ocean, int r, int c){
        ocean[r][c] = true;

        for(int[] direction : DIRECTIONS){
            int nr = r + direction[0];
            int nc = c + direction[1];
            if(nr >= 0 && nr < rows && nc >= 0 && nc < columns && !ocean[nr][nc] && heights[nr][nc] >= heights[r][c]){
                dfs(heights, ocean, nr, nc);
            }
        }
    }
}
```

### Complexity Analysis
Let `m` be the number of rows and `n` be the number of columns in the grid.

-   **Time Complexity:** O(m * n)
    -   The algorithm visits each cell at most twice (once for the Pacific DFS and once for the Atlantic DFS).

-   **Space Complexity:** O(m * m)
    -   The space is dominated by the two boolean grids (`pacific` and `atlantic`) used to store reachability. The depth of the recursion stack can also reach `m * n` in the worst case.
