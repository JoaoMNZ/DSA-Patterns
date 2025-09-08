# <a href="https://leetcode.com/problems/generate-parentheses/" target="_blank">22. Generate Parentheses</a>

### Problem Statement
Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

### Approach
This is a classic problem for a **Backtracking** algorithm. The main idea is to build the string of parentheses step-by-step, adding one character at a time. We explore all possible paths, and if a path leads to an invalid state, we "backtrack" (or undo our last choice) and try a different one.

To do this, we need to follow two simple rules:

1.  We can add an **open parenthesis `(`** at any time, as long as we still have some available to use.
2.  We can only add a **close parenthesis `)`** if it doesn't create an invalid sequence. This means we must have already placed more open parentheses than close ones. A simple way to check this is to ensure that the number of available close parentheses is greater than the number of available open ones (`close > open`).

Our **base case** (the condition to stop) is when we have used up all available open and close parentheses. At that point, we have formed a valid combination, so we add it to our results and backtrack to find more.

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
-   **Time Complexity:** O(4ⁿ / √n)
    -   The time complexity is determined by the total number of valid combinations the algorithm must generate. This quantity is defined by the n-th **Catalan number**, which results in an exponential runtime.

-   **Space Complexity:** O(n)
    -   The space is determined by the maximum recursion depth. The longest path we can build has `2n` characters (`n` open and `n` close parentheses).
