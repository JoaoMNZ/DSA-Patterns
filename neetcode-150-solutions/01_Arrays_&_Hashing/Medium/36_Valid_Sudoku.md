# <a href="https://leetcode.com/problems/valid-sudoku/" target="_blank">36. Valid Sudoku</a>

### Problem Statement
Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
1.  Each row must contain the digits 1-9 without repetition.
2.  Each column must contain the digits 1-9 without repetition.
3.  Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.

### Approach
The problem requires us to check for duplicates across three different conditions: rows, columns, and 3x3 blocks. A `HashSet` is the perfect data structure for this, as it allows us to check for duplicates in constant time.

The plan is to iterate through every cell of the board once. For each cell, we will try to add its value to three different sets:
1.  A set corresponding to its **row**.
2.  A set corresponding to its **column**.
3.  A set corresponding to its **3x3 block**.

We can use arrays of `HashSet`s to manage this easily. The `Set.add()` method in Java is very convenient because it returns `false` if you try to add an element that is already in the set.

So, for each cell, if `add()` returns `false` for any of its corresponding row, column, or block sets, we know we've found a duplicate, and the Sudoku is invalid. If we can iterate through all the cells without finding any duplicates, the board is valid.

A simple math formula, `(row / 3) * 3 + (col / 3)`, can be used to calculate a unique index (0-8) for each 3x3 block based on the cell's row and column.

### Implementation
```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean isValidSudoku(char[][] board) {
        Set<Character>[] rowSets = new HashSet[9];
        Set<Character>[] columnSets = new HashSet[9];
        Set<Character>[] blockSets = new HashSet[9];

        for (int i = 0; i < 9; i++) {
            rowSets[i] = new HashSet<>();
            columnSets[i] = new HashSet<>();
            blockSets[i] = new HashSet<>();
        }

        for (int row = 0; row < 9; row++) {
            for (int col = 0; col < 9; col++) {
                char value = board[row][col];
                if (value == '.') continue;

                int blockIndex = (row / 3) * 3 + (col / 3);

                if (!rowSets[row].add(value) || 
                    !columnSets[col].add(value) || 
                    !blockSets[blockIndex].add(value)) {
                    return false;
                }
            }
        }

        return true;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(1)
  - The Sudoku board has a fixed size (9x9). We iterate through all 81 cells once, so the number of operations is constant.

- **Space Complexity:** O(1)
  - We use a fixed amount of space for the HashSets (3 arrays of size 9). The space does not grow with any input variable `n`.
