# <a href="https://leetcode.com/problems/subtree-of-another-tree/" target="_blank">572. Subtree of Another Tree</a>

### Problem Statement
Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants.

### Approach
This problem can be solved by breaking it down into a smaller, familiar problem: checking if two trees are identical.

The core idea is recursive: For `subRoot` to be a subtree of `root`, one of three things must be true:
1.  The tree starting at `root` is identical to `subRoot`.
2.  OR, `subRoot` is a subtree of `root`'s left child.
3.  OR, `subRoot` is a subtree of `root`'s right child.

To implement this, we first need a helper function, `isSameTree`, which we already know how to write. Our main function, `isSubtree`, will then perform a traversal on the `root` tree. At each node, it will use `isSameTree` to check if the subtree starting from that node matches `subRoot`. If it doesn't find a match, it continues the search recursively on its left and right children.

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
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if (subRoot == null) {
            return true;
        }
        if (root == null) {
            return false;
        }
        if (isSameTree(root, subRoot)) {
            return true;
        }
        return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
    }

    private boolean isSameTree(TreeNode p, TreeNode q) {
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
Let `m` be the number of nodes in `root` and `n` be the number of nodes in `subRoot`.

-   **Time Complexity:** O(m * n)
    -   In the worst-case scenario, for each of the `m` nodes in the main tree, we might have to call `isSameTree`, which traverses all `n` nodes of the subtree. This results in a time complexity of `O(m * n)`.

-   **Space Complexity:** O(m)
    -   The space complexity is determined by the maximum depth of the recursion stack. In the worst case of a skewed tree, the recursion for `isSubtree` can go as deep as `m` nodes.
