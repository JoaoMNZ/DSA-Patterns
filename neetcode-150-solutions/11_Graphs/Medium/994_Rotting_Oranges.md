# <a href="https://leetcode.com/problems/rotting-oranges/" target="_blank">994. Rotting Oranges</a>

### Problem Statement
You are given an `m x n` grid where each cell can have one of three values:
-   `0` representing an empty cell,
-   `1` representing a fresh orange, or
-   `2` representing a rotten orange.

Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return `-1`.

### Approach
This problem simulates a process spreading outwards from multiple starting points, which is a perfect scenario for a **Breadth-First Search (BFS)**. We'll treat the grid like a graph where each cell is a node and adjacent cells are connected.

1.  **Initialization:**
    -   We first need to find all the initially rotten oranges. We iterate through the grid once.
    -   During this initial scan, we add the coordinates of all rotten oranges (`2`) to a `Queue`.
    -   We also count the total number of `fresh` oranges (`1`). This count is important to know when the process is complete or if some oranges are unreachable.

2.  **BFS Simulation:**
    -   We run the BFS as long as there are `fresh` oranges left and there are still rotten oranges in the `queue` to spread from.
    -   The BFS proceeds in "minutes" (levels). In each minute:
        -   We process all oranges that became rotten in the *previous* minute (all oranges currently in the queue).
        -   For each rotten orange, we check its four neighbors.
        -   If a neighbor is a fresh orange, we turn it rotten (`grid[row][col] = 2`), decrement our `fresh` count, and add its coordinates to the queue to be processed in the *next* minute.
    -   After processing all oranges for the current minute, we increment our `minute` counter.

3.  **Result:**
    -   Once the queue is empty, if our `fresh` count is `0`, it means all oranges eventually rotted, and we return the total `minutes` elapsed.
    -   If `fresh` is still greater than `0`, it means some oranges were unreachable, so we return `-1`.

### Implementation
```java
import java.util.Deque;
import java.util.ArrayDeque;

class Solution {
    private final int[][] DIRECTIONS = {{1,0}, {-1,0}, {0,1}, {0,-1}};
    private int rows;
    private int columns;

    public int orangesRotting(int[][] grid) {
        rows = grid.length;
        columns = grid[0].length;
        int fresh = 0;
        int minute = 0;

        Deque<int[]> queue = new ArrayDeque<>();
        for(int row = 0 ; row < rows ; row++){
            for(int column = 0 ; column < columns ; column++){
                if(grid[row][column] == 1){
                    fresh++;
                }else if(grid[row][column] == 2){
                    queue.addLast(new int[]{row, column});
                }
            }
        }

        while(fresh > 0 && !queue.isEmpty()){
            int size = queue.size();
            for(int i = 0 ; i < size ; i++){
                int[] cell = queue.removeFirst();
                for(int[] direction : DIRECTIONS){
                    int row = cell[0] + direction[0];
                    int column = cell[1] + direction[1];
                    if(row >= 0 && row < rows && column >= 0 && column < columns && grid[row][column] == 1){
                        queue.addLast(new int[]{row, column});
                        grid[row][column] = 2;
                        fresh--;
                    }
                }
            }
            minute++;
        }

        return fresh == 0 ? minute : -1;
    }
}
``` 

### Complexity Analysis
Let `m` be the number of rows and `n` be the number of columns in the grid.

-   **Time Complexity:** O(m * n)
    -   Each cell is visited at most a constant number of times (once during the initial scan and once during the BFS).

-   **Space Complexity:** O(m * n)
    -   In the worst-case scenario (e.g., a grid full of rotten oranges), the `queue` could potentially store all `m * n` cells.
