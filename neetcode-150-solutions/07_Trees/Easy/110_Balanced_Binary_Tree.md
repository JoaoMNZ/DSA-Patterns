# <a href="https://leetcode.com/problems/balanced-binary-tree/" target="_blank">110. Balanced Binary Tree</a>

### Problem Statement
Given a binary tree, determine if it is **height-balanced**. A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

### Approach
This problem can be solved with a single **Depth-First Search (DFS)** traversal, building upon the same logic as the "Maximum Depth of a Binary Tree" problem.

A tree is balanced if the height difference between the left and right subtrees is at most 1 for **every node** in the tree.

1.  We'll create a recursive helper function that calculates the depth of a given subtree. This function will work from the bottom of the tree upwards.
2.  For each node, the function first recursively calls itself for the left and right children to get their respective depths (`leftDepth` and `rightDepth`).
3.  Once it has the depths of both children, it checks the balance condition for the current node: `if Math.abs(leftDepth - rightDepth) > 1`.
4.  If this condition is ever met, we know the entire tree is unbalanced. We can set a global flag to `false`.
5.  Finally, the function returns the depth of the current subtree (`1 + Math.max(leftDepth, rightDepth)`) to its parent, allowing the process to continue up the tree.

After the traversal is complete, the flag will tell us if an imbalance was ever found.

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
    private boolean isBalanced = true;
    
    public boolean isBalanced(TreeNode root) {
        maxDepth(root);
        return isBalanced;
    }

    private int maxDepth(TreeNode currentNode) {
        if (currentNode == null) {
            return 0;
        }
        int leftDepth = maxDepth(currentNode.left);
        int rightDepth = maxDepth(currentNode.right);
        
        if (Math.abs(rightDepth - leftDepth) > 1) {
            isBalanced = false;
        }
        return 1 + Math.max(leftDepth, rightDepth);
    }
}
``` 

### Complexity Analysis
Let `n` be the number of nodes in the tree.

-   **Time Complexity:** O(n)
    -   The algorithm must visit each node in the tree exactly once.

-   **Space Complexity:** O(n)
    -   The space complexity is determined by the maximum depth of the recursion stack. In the worst case of a skewed tree, this is `O(n)`.
