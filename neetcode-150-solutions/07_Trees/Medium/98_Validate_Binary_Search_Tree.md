# <a href="https://leetcode.com/problems/validate-binary-search-tree/" target="_blank">98. Validate Binary Search Tree</a>

### Problem Statement
Given the `root` of a binary tree, determine if it is a valid binary search tree (BST).

A **valid BST** is defined as follows:
-   The left subtree of a node contains only nodes with keys **less than** the node's key.
-   The right subtree of a node contains only nodes with keys **greater than** the node's key.
-   Both the left and right subtrees must also be binary search trees.

### Approach
This problem can be solved with a recursive **Depth-First Search (DFS)** that validates the tree as it traverses.

The key to a valid BST is not just that `node.left.val < node.val < node.right.val`. The *entire* left subtree must be less than the node, and the *entire* right subtree must be greater. This rule applies to all ancestors, not just the immediate parent.

We can manage this by passing down valid `lowerBound` and `upperBound` constraints to each node:
1.  The `root` node can be any value (its bounds are effectively negative infinity and positive infinity).
2.  When we recursively move to a **left child**, that child must be less than its parent. So, we update the `upperBound` to be the parent's value.
3.  When we recursively move to a **right child**, that child must be greater than its parent. So, we update the `lowerBound` to be the parent's value.
4.  If any node's value violates its inherited bounds (it's not `> lowerBound` or not `< upperBound`), the tree is invalid.

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
    public boolean isValidBST(TreeNode root) {
        return dfs(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean dfs(TreeNode currentNode, long lowerBound, long upperBound){
        if(currentNode == null){
            return true;
        }
        if(currentNode.val > lowerBound && currentNode.val < upperBound){
            return dfs(currentNode.left, lowerBound, currentNode.val) &&
            dfs(currentNode.right, currentNode.val, upperBound);
        }
        return false;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of nodes in the tree.

-   **Time Complexity:** O(n)
    -   The algorithm must visit each node in the tree exactly once.

-   **Space Complexity:** O(n)
    -   The space is determined by the maximum depth of the recursion stack. In the worst case (a completely skewed tree), this is `O(n)`.
