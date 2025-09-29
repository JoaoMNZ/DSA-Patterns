# <a href="https://leetcode.com/problems/diameter-of-binary-tree/" target="_blank">543. Diameter of Binary Tree</a>

### Problem Statement
Given the `root` of a binary tree, return *the length of the **diameter** of the tree*.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`. The length of a path between two nodes is represented by the number of edges between them.

### Approach
This problem can be solved with a single **Depth-First Search (DFS)** traversal. The key is to understand what the diameter represents at any given node.

For any node, the longest path that passes *through it* as the highest point is the sum of the **depth of its left subtree** plus the **depth of its right subtree**.

The overall diameter of the tree is the maximum of these path lengths calculated for *every node*.

Our recursive algorithm will do two things at once:
1.  It will calculate the depth of a given node and return it to its parent. This is the same logic as the "Maximum Depth of a Binary Tree" problem.
2.  While calculating the depth, it will also calculate the diameter at the current node (`leftDepth + rightDepth`) and update a global `maxDiameter` variable if the current node's diameter is larger.

By the time the DFS traversal is complete, we will have checked every node and found the overall maximum diameter.

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
    private int maxDiameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        maxDepth(root);
        return maxDiameter;
    }

    private int maxDepth(TreeNode currentNode) {
        if (currentNode == null) {
            return 0;
        }
        int leftDepth = maxDepth(currentNode.left);
        int rightDepth = maxDepth(currentNode.right);
        maxDiameter = Math.max(maxDiameter, leftDepth + rightDepth);
        return 1 + Math.max(leftDepth, rightDepth);
    }
}
``` 

### Complexity Analysis
Let `n` be the number of nodes in the tree.

-   **Time Complexity:** O(n)
    -   The algorithm must visit each node in the tree exactly once.

-   **Space Complexity:** O(n)
    -   The space complexity is determined by the maximum depth of the recursion stack. In the worst case (a completely skewed tree), this is `O(n)`.
