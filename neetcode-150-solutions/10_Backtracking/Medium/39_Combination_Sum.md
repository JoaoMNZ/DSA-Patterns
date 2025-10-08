# <a href="https://leetcode.com/problems/combination-sum/" target="_blank">39. Combination Sum</a>

### Problem Statement
Given an array of **distinct** integers `candidates` and a `target` integer, return a list of all **unique combinations** of `candidates` where the chosen numbers sum to `target`. You may return the combinations in any order.

The **same number** may be chosen from `candidates` an **unlimited number of times**.

### Approach
This is a classic **Backtracking** problem. The goal is to build and explore all possible combinations that could sum up to the target. We can think of this as building a decision tree.

Our recursive function will explore different paths. At each step, we decide on the candidate at the current `index`:

1.  **Base Cases:**
    -   If the `sum` of our current combination equals the `target`, we've found a valid solution. We add a copy of it to our results and stop exploring this path.
    -   If the `sum` exceeds the `target` or we run out of candidates, this path is invalid, so we stop and backtrack.

2.  **Recursive Decisions:** For the candidate at the current `index`, we have two choices:
    -   **Choose:** We add the current candidate to our combination and make a recursive call. We stay at the **same index** to allow this number to be chosen again.
    -   **Don't Choose:** We backtrack by removing the candidate we just added. Then, we make another recursive call, but this time for the **next index** (`index + 1`), effectively deciding not to use the current candidate anymore in this path.

### Implementation
```java
import java.util.List;
import java.util.LinkedList;

class Solution {
    private List<List<Integer>> combinations = new LinkedList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        backtrack(candidates, 0, new Combination(new LinkedList<>(), 0), target);
        return combinations;
    }

    private void backtrack(int[] candidates, int index, Combination current, int target) {
        if (current.sum == target) {
            combinations.add(new LinkedList<>(current.numbers));
            return;
        }

        if (index >= candidates.length || current.sum > target) {
            return;
        }

        current.add(candidates[index]);
        backtrack(candidates, index, current, target);

        current.remove();
        backtrack(candidates, index + 1, current, target);
    }

    private static class Combination {
        List<Integer> numbers;
        int sum;

        Combination(LinkedList<Integer> numbers, int sum) {
            this.numbers = numbers;
            this.sum = sum;
        }

        void add(int num) {
            numbers.add(num);
            sum += num;
        }

        void remove() {
            int last = numbers.removeLast();
            sum -= last;
        }
    }
}
``` 

### Complexity Analysis
Let `n` be the number of candidates, `t` be the target value, and `m` be the minimum value among the candidates.

-   **Time Complexity:** O(2^t)
    -   The algorithm builds a binary decision tree (either include a number or not). The maximum depth of this tree can be up to the `target` `t` in the worst case (e.g., if `1` is a candidate). This leads to an `O(2^t)` time complexity.

-   **Space Complexity:** O(t / m)
    -   The space required is determined by the maximum depth of the recursion. This occurs by repeatedly choosing the smallest candidate (`m`) to reach the target, leading to a depth of `t / m`.
