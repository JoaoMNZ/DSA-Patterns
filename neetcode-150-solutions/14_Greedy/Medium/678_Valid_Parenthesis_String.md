# <a href="https://leetcode.com/problems/valid-parenthesis-string/" target="_blank">678. Valid Parenthesis String</a>

### Problem Statement
Given a string `s` containing only three types of characters: `'('`, `')'` and `'*'`, return `true` if `s` is valid.

The following rules define a valid string:
1.  Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
2.  Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
3.  Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
4.  `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string `""`.

### Approach
We use a **Greedy Strategy** to track the range of possible open parenthesis counts. Since the asterisk `*` introduces ambiguity, we maintain two variables:
* **`leftMax` (Optimistic):** Represents the maximum number of open parentheses we could have if we treat every `*` as `(`.
* **`leftMin` (Pessimistic):** Represents the minimum number of open parentheses we could have if we treat every `*` as `)` (or empty string if necessary).

**Logic:**
1.  Iterate through the string:
    * If `(`, increment both `leftMin` and `leftMax`.
    * If `)`, decrement both.
    * If `*`, decrement `leftMin` (treat as `)`) and increment `leftMax` (treat as `(`).
2.  **Validity Checks:**
    * If `leftMax < 0` at any point, it means we have more `)` than `(` even in the best-case scenario (turning all `*` into `(`). This is immediately invalid, return `false`.
    * If `leftMin < 0`, it means we matched too many `)` in the pessimistic scenario. However, since `*` can also be empty, we reset `leftMin` to `0` (we can't have negative open parentheses).
3.  **Final Result:** Return `leftMin == 0`. This checks if it's possible to close all open parentheses (treating `*` as `)` or empty).

### Implementation
```java
class Solution {
    public boolean checkValidString(String s) {
        int leftMin = 0;
        int leftMax = 0;
        
        for(char c : s.toCharArray()){
            if(c == '('){
                leftMin++;
                leftMax++;
            }else if(c == ')'){
                leftMin--;
                leftMax--;
            }else{
                leftMin--;
                leftMax++;
            }
            if(leftMax < 0){
                return false;
            }
            if(leftMin < 0){
                leftMin = 0;
            }
        }

        return leftMin == 0;
    }
}
```

### Complexity Analysis
Let `N` be the length of string `s`.

-   **Time Complexity:** $O(N)$
    -   We iterate through the string exactly once.
    -   Inside the loop, all operations (comparisons, increments) are constant time $O(1)$.

-   **Space Complexity:** $O(1)$
    -   We only use two integer variables (`leftMin`, `leftMax`) to store the state, regardless of the input string length.
