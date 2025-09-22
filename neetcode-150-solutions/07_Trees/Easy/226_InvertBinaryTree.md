# <a href="https://leetcode.com/problems/invert-binary-tree/" target="_blank">226. Invert Binary Tree</a>

### Problem Statement
Given the `root` of a binary tree, invert the tree, and return its root.

### Approach
This problem can be solved elegantly using a simple **Recursive** approach. The idea is to break the problem down into the smallest possible step and apply it repeatedly.

The process for any given node is simple: we swap its left and right children. Then, we trust recursion to do the exact same thing for each of those children, and so on, until we've visited the entire tree.

1.  **Base Case:** If the current node is `null`, we've reached the end of a branch, so we do nothing and return.
2.  **Recursive Step:** For the current node:
    -   Swap its `left` and `right` child nodes.
    -   Recursively call the function for the new left child.
    -   Recursively call the function for the new right child.

By starting at the root and applying this logic all the way down, the entire tree will be inverted.

### Implementation
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode() {}
 * TreeNode(int val) { this.val = val; }
 * TreeNode(int val, TreeNode left, TreeNode right) {
 * this.val = val;
 * this.left = left;
 * this.right = right;
 * }
 * }
 */
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }

        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;

        invertTree(root.left);
        invertTree(root.right);

        return root;
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   The algorithm visits each node in the tree exactly once.

-   **Space Complexity:** O(n)
    -   The space required is determined by the maximum depth of the recursion stack. In the worst case (a completely skewed tree), the recursion depth can be `n`.
