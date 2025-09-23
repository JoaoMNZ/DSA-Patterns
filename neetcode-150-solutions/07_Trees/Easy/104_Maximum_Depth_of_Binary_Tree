# <a href="https://leetcode.com/problems/maximum-depth-of-binary-tree/" target="_blank">104. Maximum Depth of Binary Tree</a>

### Problem Statement
Given the `root` of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

### Approach
This problem is solved using a **Recursive** approach. The core idea is to define the depth of any given node based on the depths of its children. The algorithm works from the bottom up.

1.  **Base Case:** The simplest case is an empty tree (a `null` node). By definition, its depth is `0`.
2.  **Recursive Step:** For any other node, its maximum depth is `1` (to count the node itself) plus the maximum depth of its largest subtree (whichever is bigger, the left or the right).

The function calls itself until it reaches the leaves. A leaf's children are `null` (depth 0), so the depth of a leaf is calculated as `1 + Math.max(0, 0)`, which is `1`. This result is then returned to its parent node. Each parent receives the depths from its children, takes the maximum value, adds 1, and passes that result up the call stack until the final calculation is done at the root.

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
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   The algorithm must visit every node in the tree exactly once.

-   **Space Complexity:** O(n)
    -   The space required is determined by the maximum depth of the recursion stack. In the worst case (a completely skewed tree), the recursion depth can be `n`.