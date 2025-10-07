# <a href="https://leetcode.com/problems/subsets/" target="_blank">78. Subsets</a>

### Problem Statement
Given an integer array `nums` of **unique** elements, return all possible **subsets** (the power set). The solution set must not contain duplicate subsets. Return the solution in any order.

### Approach
This is a classic problem for a **Backtracking** algorithm, where we build every possible combination step by step.

The core idea is that for each number in the input array, we have two choices:
1.  **Include** the number in our current subset.
2.  **Exclude** the number from our current subset.

We can visualize this as a decision tree. Starting with an empty subset, we consider the first number. This creates two branches: one with the number and one without. Each of these branches then does the same for the second number, and so on.

Our recursive `dfs` function will handle this branching.
-   The **base case** is when we have considered every number in the input array. At this point, the current `subset` is a complete and valid solution, so we add a copy of it to our final result list.
-   The **recursive step** involves making two calls for each number: one where we add the number to the subset before the call, and another where we don't (after backtracking by removing the number).

### Implementation
```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    private List<List<Integer>> result = new ArrayList<>();

    public List<List<Integer>> subsets(int[] nums) {
        dfs(new ArrayList<>(), nums, 0);
        return result;
    }

    private void dfs(List<Integer> subset, int[] nums, int i) {
        if (i >= nums.length) {
            result.add(new ArrayList<>(subset));
            return;
        }

        subset.add(nums[i]);
        dfs(subset, nums, i + 1);
        subset.remove(subset.size() - 1);
        dfs(subset, nums, i + 1);
    }
}
``` 

### Complexity Analysis
Let `n` be the number of elements in the array.

-   **Time Complexity:** O(n * 2ⁿ)
    -   For each of the `n` elements, we have two choices (include or exclude). This creates a total of `2ⁿ` possible subsets. For each of these subsets, we create a copy to add to our result list, which can take up to `O(n)` time. This leads to a total time complexity of `O(n * 2ⁿ)`.

-   **Space Complexity:** O(n)
    -   The auxiliary space is determined by the maximum depth of the recursion, which is `n`. This space is used for the recursion stack and for the `subset` list we are building. The space required for the final `result` list is not counted.
