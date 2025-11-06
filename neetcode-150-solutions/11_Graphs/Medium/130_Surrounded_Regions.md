# <a href="https://leetcode.com/problems/surrounded-regions/" target="_blank">130. Surrounded Regions</a>

### Problem Statement
Given an `m x n` matrix `board` containing `'X'` and `'O'`, capture all regions that are 4-directionally surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

### Approach
Instead of trying to find the 'O's that *are* surrounded, it's much easier to find the 'O's that *are not* surrounded and protect them. An 'O' is not surrounded if it is on the border of the grid or if it can be reached from an 'O' on the border.

We can solve this using a **Depth-First Search (DFS)** starting from all 'O's on the borders.

1.  **Mark Safe 'O's:** We iterate through all cells on the four borders (top/bottom rows and left/right columns). If we find an 'O', we start a recursive DFS from that cell.
2.  **DFS Logic:** The DFS will find all other 'O's connected to this border 'O'. We mark all of these "safe" cells with a temporary character, like `'T'`, to show they've been visited and are connected to the border.
3.  **Flip Cells:** After running the DFS on all border cells, we iterate through the entire grid one last time.
    -   If a cell is still an `'O'`, it means it was not reachable from any border and is therefore surrounded. We flip it to `'X'`.
    -   If a cell is our temporary `'T'`, it means it *was* connected to a border and is safe. We flip it back to `'O'`.

### Implementation
```java
class Solution {
    int m = 0;
    int n = 0;
    int[][] directions = new int[][]{ {0, 1}, {0, -1}, {1, 0}, {-1, 0} };
    
    public void solve(char[][] board) {
        m = board.length;
        n = board[0].length;
        
        for(int r = 0 ; r < m; r++){
            dfs(board, r, 0);
            dfs(board, r, n - 1);
        }

        for(int c = 0 ; c < n ; c++){
            dfs(board, 0, c);
            dfs(board, m - 1, c);
        }

        for(int r = 0 ; r < m ; r++){
            for(int c = 0 ; c < n ; c++){
                if(board[r][c] == 'T'){
                    board[r][c] = 'O';
                }else{
                    board[r][c] = 'X';
                }
            }
        }
    }

    public void dfs(char[][] board, int r, int c){
        if(r < 0 || r >= m || c < 0 || c >= n || board[r][c] != 'O')
            return;
        
        board[r][c] = 'T';
        for(int[] direction : directions){
            dfs(board, r + direction[0], c + direction[1]);
        }
    }
}
``` 

### Complexity Analysis
Let `m` be the number of rows and `n` be the number of columns.

-   **Time Complexity:** O(m * n)
    -   In the worst case, the DFS visits every cell on the grid (e.g., a grid of all 'O's). The final loop to flip the cells also iterates through all `m * n` cells.

-   **Space Complexity:** O(m * n)
    -   The space is determined by the maximum depth of the recursion stack. In the worst case (a grid full of 'O's), the recursion stack can grow to `O(m * n)`.
