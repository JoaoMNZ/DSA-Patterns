# <a href="https://leetcode.com/problems/generate-parentheses/" target="_blank">22. Generate Parentheses</a>

### Problem Statement
Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

### Approach
This is a classic problem for a **Backtracking** algorithm. The main idea is to build the string of parentheses step-by-step. We can only add a parenthesis if it follows the rules of being "well-formed."

We track the number of available `open` and `close` parentheses we still need to add.

1.  **Base Case:** If we have used all `open` and `close` parentheses (`open == 0 && close == 0`), we have a valid combination, so we add it to our result list.
2.  **Recursive Steps:**
    -   **Adding `(`:** We can add an open parenthesis as long as we have any available (`open > 0`).
    -   **Adding `)`:** We can only add a close parenthesis if it would be valid, which means there must be an open parenthesis in our current string that hasn't been closed yet. We track this by checking if `close > open`.

We explore these choices, and after a recursive call returns (backtracks), we remove the character to explore other paths.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        StringBuilder current = new StringBuilder();
        backtrack(result, current, n, n);
        return result;
    }

    private void backtrack(List<String> result, StringBuilder current, int open, int close) {
        if (open == 0 && close == 0) {
            result.add(current.toString());
            return;
        }

        if (open > 0) {
            current.append('(');
            backtrack(result, current, open - 1, close);
            current.deleteCharAt(current.length() - 1);
        }

        if (close > open) {
            current.append(')');
            backtrack(result, current, open, close - 1);
            current.deleteCharAt(current.length() - 1);
        }
    }
}
``` 

### Complexity Analysis
Let `n` be the number of parenthesis pairs.

-   **Time Complexity:** O(4ⁿ / √n)
    -   The time complexity is determined by the total number of valid combinations the algorithm must generate. This quantity is defined by the n-th Catalan number, which results in an exponential runtime.

-   **Space Complexity:** O(n)
    -   The space is determined by the maximum depth of the recursion stack. The longest path we can build has `2n` characters (`n` open and `n` close parentheses), which simplifies to O(n).
