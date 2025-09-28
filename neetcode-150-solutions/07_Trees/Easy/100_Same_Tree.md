# <a href="https://leetcode.com/problems/same-tree/" target="_blank">100. Same Tree</a>

### Problem Statement
Given the roots of two binary trees, `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

### Approach
This problem is a perfect fit for a simple **Recursive** traversal. The core idea is to traverse both trees at the same time, comparing the nodes at each position.

The logic can be broken down into a few simple checks:

1.  **Base Case 1 (Match):** If both nodes we are comparing (`p` and `q`) are `null`, it means we've reached the end of a branch on both trees at the same time. This part of the tree is identical, so we return `true`.
2.  **Base Case 2 (Mismatch):** If *only one* of the nodes is `null`, or if the nodes' values are different, then the trees are not identical. In any of these cases, we return `false`.
3.  **Recursive Step:** If the current nodes are the same (not null and have equal values), we then need to check their children. The trees as a whole are only identical if **the left subtrees are identical AND the right subtrees are identical**. We verify this by making two recursive calls.

### Implementation
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        if (p == null || q == null || p.val != q.val) {
            return false;
        }
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
``` 

### Complexity Analysis
Let `n` be the number of nodes in the tree.

-   **Time Complexity:** O(n)
    -   In the worst case (the trees are identical), the algorithm must visit each node in both trees exactly once.

-   **Space Complexity:** O(n)
    -   The space complexity is determined by the maximum depth of the recursion stack. In the worst case (a completely skewed tree), this is `O(n)`.
