# <a href="https://leetcode.com/problems/merge-triplets-to-form-target-triplet/" target="_blank">1899. Merge Triplets to Form Target Triplet</a>

### Problem Statement
A **triplet** is an array of three integers. You are given a 2D integer array `triplets`, where `triplets[i] = [ai, bi, ci]`, and an integer array `target = [x, y, z]`.

You want to form `target` by taking the **element-wise maximum** of some indices. You can select any indices (possibly none) and apply the operation.
* For example, if you choose triplets `[2, 5, 3]` and `[1, 7, 5]`, the result is `[max(2, 1), max(5, 7), max(3, 5)] = [2, 7, 5]`.

Return `true` if it is possible to form the `target` triplet `[x, y, z]`, or `false` otherwise.

### Approach
We can use a **Greedy Strategy**. The key insight is that the operation is strictly increasing (taking the maximum).
1.  **Filter Invalid Triplets:** If a triplet has *any* component larger than the corresponding component in the `target`, it is useless. Including it would permanently make the resulting triplet larger than the target at that position. We must skip these "invalid" triplets.
2.  **Accumulate Valid Max:** For the remaining "valid" triplets (where all components are $\le$ target), we can safely merge them. We maintain a `start` array (or boolean flags) to track the maximum value found so far for each of the three positions.
3.  **Check Result:** After iterating through all triplets, if the maximums we managed to construct match the `target` exactly, return `true`.

### Implementation
```java
class Solution {
    public boolean mergeTriplets(int[][] triplets, int[] target) {
        int[] start = new int[]{0, 0, 0};
        for(int[] triplet : triplets){
            if(triplet[0] > target[0] || triplet[1] > target[1] || triplet[2] > target[2]){
                continue;
            }
            start = new int[]{Math.max(start[0], triplet[0]), Math.max(start[1], triplet[1]), Math.max(start[2], triplet[2])};
        }
        return start[0] == target[0] && start[1] == target[1] && start[2] == target[2];
    }
}
```

### Complexity Analysis
Let `N` be the number of triplets in the input array.

-   **Time Complexity:** $O(N)$
    -   We iterate through the `triplets` array exactly once.
    -   Inside the loop, all operations (comparisons, `Math.max`) are performed on triplets of fixed size 3, so they take constant time $O(1)$.

-   **Space Complexity:** $O(1)$
    -   The algorithm uses a constant amount of extra space (an array of size 3) regardless of the number of triplets in the input.