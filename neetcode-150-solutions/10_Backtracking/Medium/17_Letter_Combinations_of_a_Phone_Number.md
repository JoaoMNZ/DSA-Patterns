# <a href="https://leetcode.com/problems/letter-combinations-of-a-phone-number/" target="_blank">17. Letter Combinations of a Phone Number</a>

### Problem Statement
Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

### Approach
This problem asks for all possible combinations, which is a classic use case for a **Backtracking** algorithm. The strategy is to build each combination character by character.

Our recursive function will explore the decision tree of possibilities:
1.  **Base Case:** The recursion stops when the length of our `currentCombination` is equal to the length of the input `digits` string. At this point, we've formed a complete, valid combination, so we add it to our results.
2.  **Recursive Step:** For the current digit we're considering:
    -   We get the string of possible letters from our mapping (e.g., '2' -> "abc").
    -   We then loop through each of these letters.
    -   For each letter, we **choose** it by appending it to our `currentCombination`.
    -   We **explore** further possibilities by making a recursive call for the next digit.
    -   After the recursive call returns, we **unchoose** the letter (backtrack) by deleting it. This allows us to explore the other letters for the current digit.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    private List<String> combinations = new ArrayList<>();
    
    public List<String> letterCombinations(String digits) {
        String[] digitToChar = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        backtrack(digitToChar, digits, 0, new StringBuilder());
        return combinations;
    }

    private void backtrack(String[] digitToChar, String digits, int index, StringBuilder currentCombination) {
        if (currentCombination.length() == digits.length()) {
            combinations.add(currentCombination.toString());
            return;
        }

        String letters = digitToChar[digits.charAt(index) - '0'];
        for (char letter : letters.toCharArray()) {
            currentCombination.append(letter);
            backtrack(digitToChar, digits, index + 1, currentCombination);
            currentCombination.deleteCharAt(currentCombination.length() - 1);
        }
    }
}
``` 

### Complexity Analysis
Let `n` be the length of the input `digits` string.

-   **Time Complexity:** O(n * 4ⁿ)
    -   In the worst case, a digit can map to 4 letters. For each valid combination, we must create a string of length `n`, leading to the `n * 4ⁿ` complexity.

-   **Space Complexity:** O(n)
    -   The space is determined by the maximum depth of the recursion stack, which is equal to the length of the input string, `n`.
