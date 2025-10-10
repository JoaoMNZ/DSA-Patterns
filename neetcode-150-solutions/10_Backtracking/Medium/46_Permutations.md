# <a href="https://leetcode.com/problems/permutations/" target="_blank">46. Permutations</a>

### Problem Statement
Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.

### Approach
The goal is to build every possible ordering of the numbers. A key rule for permutations is that each number can only be used once per combination.

To keep track of which numbers are already in our current permutation, a boolean `used` array is a simple and efficient choice.

The recursive process works as follows:
1.  **Base Case:** If the size of our current permutation is equal to the size of the input array, we have formed a complete, valid permutation. We add a copy of it to our results and stop this path.
2.  **Recursive Step:** We loop through all the numbers in the original input array.
    -   If a number is already marked as `used`, we skip it to avoid duplicates in the current permutation.
    -   Otherwise, we "choose" the number: add it to our permutation and mark its position as `true` in the `used` array.
    -   We then make a recursive call to continue building the permutation from this new state.
    -   After the recursive call returns, we "unchoose" the number (this is the backtrack step): we remove it from the permutation and reset its position to `false` in the `used` array, allowing it to be used in different positions in other permutations.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    private List<List<Integer>> permutations = new ArrayList<>();

    public List<List<Integer>> permute(int[] nums) {
        backtrack(nums, new ArrayList<>(), new boolean[nums.length]);
        return permutations;
    }

    private void backtrack(int[] nums, List<Integer> currentPermutation, boolean[] used) {
        if (currentPermutation.size() == nums.length) {
            permutations.add(new ArrayList<>(currentPermutation));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (used[i]) {
                continue;
            }

            used[i] = true;
            currentPermutation.add(nums[i]);
            backtrack(nums, currentPermutation, used);
            
            used[i] = false;
            currentPermutation.remove(currentPermutation.size() - 1);
        }
    }
}
``` 

### Complexity Analysis
Let `n` be the size of the input array.

-   **Time Complexity:** O(n * n!)
    -   There are `n!` (n factorial) possible permutations for an array of `n` distinct elements. For each of these `n!` permutations, the algorithm creates a new list of size `n` to add to the result.

-   **Space Complexity:** O(n)
    -   The auxiliary space is determined by the `used` array (size `n`) and the depth of the recursion stack (which also goes up to `n`).
