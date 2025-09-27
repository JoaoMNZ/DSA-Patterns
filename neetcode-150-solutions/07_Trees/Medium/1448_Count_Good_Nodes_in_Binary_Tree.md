# <a href="https://leetcode.com/problems/count-good-nodes-in-binary-tree/" target="_blank">1448. Count Good Nodes in Binary Tree</a>

### Problem Statement
Given a binary tree `root`, a node `X` in the tree is named **good** if in the path from root to `X` there are no nodes with a value greater than `X`.

Return the number of good nodes in the binary tree.

### Approach
This problem can be solved with a **Depth-First Search (DFS)**. The key is to pass down the maximum value found so far on the path from the root to the current node.

Our recursive function will traverse the tree and keep track of the maximum value encountered on the path to the current node (`maxSoFar`).

1.  A node is considered "good" if its value is greater than or equal to `maxSoFar`.
2.  If the current node is good, we increment a counter.
3.  When we recursively call the function for the children, we must pass them the new maximum for their path. This will be the greater of `maxSoFar` and the current node's value.
4.  The initial call for the root will use a very small number (like `Integer.MIN_VALUE`) as the starting `maxSoFar` to ensure the root itself is always counted as a good node.

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
    private int goodNodesQuantity = 0;
    
    public int goodNodes(TreeNode root) {
        dfs(root, Integer.MIN_VALUE);
        return goodNodesQuantity;
    }

    public void dfs(TreeNode currentNode, int maxSoFar) {
        if (currentNode == null) {
            return;
        }

        if (currentNode.val >= maxSoFar) {
            goodNodesQuantity++;
            maxSoFar = currentNode.val;
        }

        dfs(currentNode.left, maxSoFar);
        dfs(currentNode.right, maxSoFar);
    }
}
``` 

### Complexity Analysis
Let `n` be the number of nodes in the tree.

-   **Time Complexity:** O(n)
    -   The algorithm must visit each node in the tree exactly once.

-   **Space Complexity:** O(n)
    -   The space complexity is determined by the maximum depth of the recursion stack. In the worst case (a completely skewed tree), this is `O(n)`. 
