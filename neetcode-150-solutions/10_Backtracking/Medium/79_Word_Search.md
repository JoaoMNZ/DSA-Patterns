# <a href="https://leetcode.com/problems/word-search/" target="_blank">79. Word Search</a>

### Problem Statement
Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

### Approach
This problem is a classic fit for a **Backtracking** algorithm using a Depth-First Search (DFS) approach. The main idea is to treat the grid as a graph and try to find a path that spells out the `word`.

1.  **Starting Point:** We iterate through every cell in the grid. If a cell's character matches the first letter of the `word`, we use it as a starting point for our search.
2.  **Recursive Search (DFS):** From a starting cell, our recursive function will try to find the rest of the word.
    -   **Base Cases:** The search stops if we go out of bounds, if the character in the current cell doesn't match the required character in the `word`, or if we have successfully found all the characters in the `word`.
    -   **Exploration:** If the current cell is a match, we explore its four neighbors (up, down, left, right) to find the next character of the `word`.
    -   **Marking (Crucial Step):** To ensure we don't use the same letter cell twice in one path, we "mark" the current cell as visited (e.g., by changing its character) before making the recursive calls.
    -   **Backtracking:** After exploring all neighbors from a cell, we must "un-mark" it (change the character back). This is the backtracking step, and it's essential so that this cell can be used in other potential paths that might start from a different initial cell.

### Implementation
```java
class Solution {
    private int rows;
    private int columns;

    public boolean exist(char[][] board, String word) {
        rows = board.length;
        columns = board[0].length;

        for (int row = 0; row < rows; row++) {
            for (int column = 0; column < columns; column++) {
                if (backtrack(board, word, row, column, 0)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean backtrack(char[][] board, String word, int row, int column, int index) {
        if (index == word.length()) {
            return true;
        }

        if (row < 0 || column < 0 || row >= rows || column >= columns || board[row][column] != word.charAt(index)) {
            return false;
        }

        board[row][column] = '0';
        boolean found = backtrack(board, word, row + 1, column, index + 1) ||
                        backtrack(board, word, row - 1, column, index + 1) ||
                        backtrack(board, word, row, column + 1, index + 1) ||
                        backtrack(board, word, row, column - 1, index + 1);

        board[row][column] = word.charAt(index);

        return found;
    }
}
``` 

### Complexity Analysis
Let `m` be the number of cells in the boardboard  and `n` be the length of the `word`.

-   **Time Complexity:** O(m * 3^n)
    -   We may start the search from any of the m cells. In the worst case, for each cell in the path, the backtracking function branches out to 3 directions (we can't go back to the cell we came from).

-   **Space Complexity:** O(L)
    -   The auxiliary space is determined by the maximum depth of the recursion, which is equal to the length of the `word`.
