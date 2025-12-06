# <a href="https://leetcode.com/problems/walls-and-gates/" target="_blank">286. Walls and Gates</a> (Islands and Treasure)

### Problem Statement
You are given a `m x n` 2D grid initialized with these three possible values:
-   `-1`: A water cell (or wall) that cannot be traversed.
-   `0`: A treasure chest (or gate).
-   `INF`: A land cell (or empty room) that can be traversed. We use the integer `2^31 - 1 = 2147483647` to represent `INF`.

Fill each land cell with the distance to its nearest treasure chest. If a land cell cannot reach a treasure chest, then the value should remain `INF`.

Assume the grid can only be traversed up, down, left, or right. Modify the grid in-place.

### Approach
Instead of running a Breadth-First Search (BFS) from every single empty land cell to find the nearest. We start a **Multi-Source BFS** simultaneously from all treasure chests.

1.  **Initialization:** We iterate through the entire grid. We add the coordinates of all **treasure chests (`0`)** to a Queue.
2.  **BFS Traversal:** We process the queue. For every cell we pop, we look at its four neighbors (up, down, left, right).
3.  **Update Logic:** If a neighbor is an **empty land cell (`INF`)**, it means we have found the shortest path to this cell for the first time.
    -   We update its value to be the distance of the current cell + 1.
    -   We add this neighbor to the queue to continue the search.
    -   *Note:* We ignore water cells (`-1`) and cells that already have a computed distance (value `< INF`), as we have already found a shorter or equal path to them.

This ensures that we fill the grid layer by layer, guaranteeing the shortest distance for every cell.

### Implementation
```java
import java.util.Deque;
import java.util.ArrayDeque;

class Solution {
    public void islandsAndTreasure(int[][] grid) {
        Deque<int[]> queue = new ArrayDeque<>();

        int rows = grid.length;
        int cols = grid[0].length;

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 0) {
                    queue.offer(new int[]{i, j});
                }
            }
        }

        int[][] directions = new int[][]{ {-1, 0}, {1, 0}, {0, -1}, {0, 1} };

        while (!queue.isEmpty()) {
            int[] cell = queue.poll();
            int row = cell[0];
            int col = cell[1];

            for (int[] dir : directions) {
                int nRow = row + dir[0];
                int nCol = col + dir[1];

                if (nRow < 0 || nRow >= rows || nCol < 0 || nCol >= cols || grid[nRow][nCol] != Integer.MAX_VALUE) {
                    continue;
                }
                
                grid[nRow][nCol] = grid[row][col] + 1;
                queue.offer(new int[]{nRow, nCol});
            }
        }
    }
}
``` 

### Complexity Analysis
Let `m` be the number of rows and `n` be the number of columns.

-   **Time Complexity:** O(m * n)
    -   We iterate through the grid once to find the chests. Then, during the BFS, each cell is added to the queue and processed at most once.

-   **Space Complexity:** O(m * n)
    -   The space is primarily used by the queue. In the worst case (e.g., a grid full of chests, the queue might store a number of cells proportional to the size of the grid.
