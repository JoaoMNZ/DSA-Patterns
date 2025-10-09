# <a href="https://leetcode.com/problems/combination-sum-ii/" target="_blank">40. Combination Sum II</a>

### Problem Statement
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`. Each number in `candidates` may only be used **once** in the combination.

Note: The solution set must not contain duplicate combinations.

### Approach
This is a **Backtracking** problem with an added challenge: we must handle duplicate numbers in the input and ensure our result contains only unique combinations.

The strategy to avoid duplicates has two key parts:
1.  **Sort the Input:** First, we sort the `candidates` array. This is crucial because it groups all identical numbers together, making them easy to identify and skip.
2.  **Skip Duplicates:** In our recursive function, we loop through the candidates to build a combination. To prevent duplicate combinations (like `[1, 7]` and another `[1, 7]` if the input has two `7`s), we add a rule: after considering a number, we skip all identical numbers that immediately follow it *at the same level of decision*. This is handled by the check `if (i > start && candidates[i] == candidates[i-1])`.

The rest of the logic is a standard backtracking approach where we add a candidate, recurse, and then backtrack by removing it. We also include an optimization: since the array is sorted, if a number is already too large to be added, we can stop searching from that point on.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;
import java.util.Arrays;

class Solution {
    private List<List<Integer>> combinations = new ArrayList<>();

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        backtrack(candidates, 0, new ArrayList<>(), target);
        return combinations;
    }

    private void backtrack(int[] candidates, int start, List<Integer> currentCombination, int target) {
        if (target == 0) {
            combinations.add(new ArrayList<>(currentCombination));
            return;
        }

        for (int i = start; i < candidates.length; i++) {
            if (i > start && candidates[i] == candidates[i - 1]) {
                continue;
            }

            if (candidates[i] > target) {
                break;
            }
            
            currentCombination.add(candidates[i]);
            backtrack(candidates, i + 1, currentCombination, target - candidates[i]);
            currentCombination.remove(currentCombination.size() - 1);
        }
    }
}
``` 

### Complexity Analysis
Let `n` be the number of candidates.

-   **Time Complexity:** O(n * 2^n)
    -   In the worst case, the algorithm explores `2^n` combinations (each element is either included or not). For each valid combination, we create a copy, which can take up to `O(n)` time.

-   **Space Complexity:** O(n)
    -   The space is determined by the maximum depth of the recursion, which can be up to `n` (the number of candidates).
