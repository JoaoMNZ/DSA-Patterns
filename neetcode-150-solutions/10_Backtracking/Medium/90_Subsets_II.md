# <a href="https://leetcode.com/problems/subsets-ii/" target="_blank">90. Subsets II</a>

### Problem Statement
Given an integer array `nums` that may contain duplicates, return all possible subsets (the power set). The solution set must not contain duplicate subsets. Return the solution in any order.

### Approach
This problem is an extension of "Subsets I," with the added challenge of handling duplicate numbers in the input. To avoid generating duplicate subsets, we can use a **Backtracking** approach with a simple rule.

1.  **Sort the Input:** First, we sort the `nums` array. This is a crucial step because it groups all identical numbers together, making it easy to identify and handle them.
2.  **Backtrack Logic:** We build our subsets recursively. The key difference from the original "Subsets" problem is the logic inside our loop:
    -   At each step, we first add the current subset to our results.
    -   Then, we loop through the remaining numbers to add a new element and recurse.
    -   **The Skip Rule:** To prevent duplicates, we add a condition: `if (i > start && nums[i] == nums[i - 1])`. This check ensures that we only use the first occurrence of a number at a specific level of our decision tree. If we encounter a duplicate number, we skip it, which prunes the branches that would lead to duplicate subsets.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;
import java.util.Arrays;

class Solution {
    List<List<Integer>> subsets = new ArrayList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        backtrack(nums, 0, new ArrayList<>());
        return subsets;
    }

    private void backtrack(int[] nums, int start, List<Integer> currentSubset) {
        subsets.add(new ArrayList<>(currentSubset));

        for (int i = start; i < nums.length; i++) {
            if (i > start && nums[i] == nums[i - 1]) {
                continue;
            }

            currentSubset.add(nums[i]);
            backtrack(nums, i + 1, currentSubset);
            
            currentSubset.remove(currentSubset.size() - 1);
        }
    }
}
``` 

### Complexity Analysis
Let `n` be the number of elements in the array.

-   **Time Complexity:** O(n * 2ⁿ)
    -   The backtracking algorithm generates `2ⁿ` subsets in the worst case. For each subset, we create a copy, which can take up to `O(n)` time.

-   **Space Complexity:** O(n)
    -   The auxiliary space is determined by the maximum depth of the recursion, which is `n`.
