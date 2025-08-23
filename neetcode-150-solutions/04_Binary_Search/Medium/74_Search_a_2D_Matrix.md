# <a href="https://leetcode.com/problems/search-a-2d-matrix/description/" target="_blank">74. Search a 2D Matrix</a>

### Problem Statement
You are given an `m x n` integer matrix `matrix` with the following two properties:
- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` if `target` is in `matrix` or `false` otherwise.
You must write a solution in `O(log(m * n))` time complexity.

### Approach
This problem can be broken down into two parts, both solvable with **Binary Search**.

**Part 1: Find the Correct Row**
First, we need to find which row might contain the target. We can use Binary Search on the rows themselves.
- In each step, we pick a middle row.
- We check if the `target` value could possibly be in this row by looking at its first and last elements.
  - If the `target` is smaller than the first element of the middle row, it must be in a row above. We can ignore the bottom half.
  - If the `target` is larger than the last element of the middle row, it must be in a row below. We can ignore the top half.
  - Otherwise, the `target` must be within this row's range, and we've found the correct row to search.

**Part 2: Find the Target in the Row**
Once we've identified the correct row, we perform a second, standard Binary Search on the columns of that row. This will find the `target` if it exists.

If the first search completes without finding a potential row, or the second search completes without finding the `target`, then the value is not in the matrix.

### Implementation
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int top = 0;
        int bottom = matrix.length - 1;

        while (top <= bottom) {
            int midRow = (top + bottom) / 2;
            int rowStart = matrix[midRow][0];
            int rowEnd = matrix[midRow][matrix[midRow].length - 1];

            if (rowStart > target ) {
                bottom = midRow - 1;
            } else if (rowEnd < target) {
                top = midRow + 1;
            } else {
                int left = 0;
                int right = matrix[midRow].length - 1;
                while (left <= right) {
                    int midCol = (left + right) / 2;
                    if (matrix[midRow][midCol] < target) {
                        left = midCol + 1;
                    } else if (matrix[midRow][midCol] > target) {
                        right = midCol - 1;
                    } else {
                        return true;
                    }
                }
                return false;
            }
        }
        return false;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(log(n*m))
  - We perform two separate Binary Search operations. The first search on `m` rows takes `O(log m)` time. The second search on `n` columns takes `O(log n)` time.

- **Space Complexity:** O(1)
  - The algorithm uses a fixed number of variables for pointers. The space required does not grow with the size of the input matrix.
