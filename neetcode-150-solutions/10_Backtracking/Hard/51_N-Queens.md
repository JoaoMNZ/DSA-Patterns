# <a href="https://leetcode.com/problems/n-queens/" target="_blank">51. N-Queens</a>

### Problem Statement
The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *all distinct solutions to the **n-queens puzzle***. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

### Approach
A Queen can attack horizontally, vertically, and diagonally.
1.  **Rows:** We avoid row conflicts automatically by placing exactly one queen per row and then recursing to the next row (`backtrack(row + 1, n)`).
2.  **Columns:** We maintain a boolean array `columns` to track which columns are already occupied.
3.  **Diagonals:** This is the trickiest part. On a grid, diagonals share specific mathematical properties:
    * **Positive Diagonals (Anti-diagonal /):** For every cell `(r, c)` on this diagonal, the sum `r + c` is constant.
    * **Negative Diagonals (Main diagonal \):** For every cell `(r, c)` on this diagonal, the difference `r - c` is constant. To map this to an array index (since `r - c` can be negative), we add an offset `n`: `r - c + n`.



We use three boolean arrays (`columns`, `posDiag`, `negDiag`) to check if a position is safe in $O(1)$ time. This allows us to perform efficient backtracking by "pruning" invalid branches immediately.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;
import java.util.Arrays;

class Solution {
    List<List<String>> result = new ArrayList<>();
    char[][] board;
    boolean[] columns, posDiag, negDiag;

    public List<List<String>> solveNQueens(int n) {
        columns = new boolean[n];
        posDiag = new boolean[2 * n];
        negDiag = new boolean[2 * n];
        board = new char[n][n];

        for(char[] row : board){
            Arrays.fill(row, '.');
        }

        backtrack(0, n);
        return result;
    }

    public void backtrack(int row, int n){
        if (row >= n) {
            List<String> snapshot = new ArrayList<>();
            for (char[] r : board) {
                snapshot.add(new String(r));
            }
            result.add(snapshot);
            return;
        }

        for(int column = 0 ; column < n ; column++){
            if(columns[column] || posDiag[row + column] || negDiag[row - column + n]){
                continue;
            }

            columns[column] = true;
            posDiag[row + column] = true;
            negDiag[row - column + n] = true;
            board[row][column] = 'Q';

            backtrack(row + 1, n);

            columns[column] = false;
            posDiag[row + column] = false;
            negDiag[row - column + n] = false;
            board[row][column] = '.';
        }
    }
}
```

### Complexity Analysis
Let `N` be the size of the board (number of queens).

-   **Time Complexity:** $O(N!)$
    -   In the first row, we have $N$ choices. In the second row, we have roughly $N-2$ choices (due to column and diagonal constraints), and so on. The upper bound is $N!$.

-   **Space Complexity:** $O(N^2)$
    -   **Board:** We store the board state which takes $O(N^2)$.
    -   **Auxiliary Arrays:** The boolean arrays (`columns`, `posDiag`, `negDiag`) take $O(N)$.
    -   **Recursion Stack:** The maximum depth of the recursion tree is $N$, so stack space is $O(N)$.
