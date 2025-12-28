# <a href="https://leetcode.com/problems/binary-tree-maximum-path-sum/" target="_blank">124. Binary Tree Maximum Path Sum</a>

### Problem Statement
A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** is the sum of the node's values in the path.

Given the `root` of a binary tree, return *the maximum path sum of any **non-empty** path*.

### Approach
The key to solving this problem is understanding that for any given node, the "Maximum Path" involving that node can take two forms:

1.  **The "Split" Path (Arch):** The path goes up from the left child, through the current node, and down to the right child. This path **cannot** be extended further up to the parent node (because a path cannot branch). This is the value we use to update our global maximum.
    * `Split Sum = Node.val + LeftMax + RightMax`
    
2.  **The "Continuing" Path (Branch):** The path comes from one child (left or right) and continues up to the parent. This is the value we **return** from our recursive function.
    * `Return Value = Node.val + max(LeftMax, RightMax)`



**Algorithm:**
1.  **Post-Order Traversal:** We calculate the max path values from the bottom up.
2.  **Ignore Negatives:** If a subtree (left or right) returns a negative sum, we should ignore it (treat it as `0`). Adding a negative value would only decrease our total sum.
3.  **Update Global Max:** At every node, we calculate the "Split Path" sum (`val + left + right`) and compare it against our global `maxPath`.
4.  **Return:** We return the "Continuing Path" sum to the parent node.

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
    int maxPath = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        dfs(root);
        return maxPath;
    }

    public int dfs(TreeNode curr){
        if(curr == null){
            return 0;
        }
        
        int left = Math.max(0, dfs(curr.left));
        int right = Math.max(0, dfs(curr.right));

        maxPath = Math.max(maxPath, curr.val + left + right);

        return curr.val + Math.max(left, right);
    }
}
``` 

### Complexity Analysis
Let `N` be the number of nodes in the tree.

-   **Time Complexity:** $O(N)$
    -   We perform a Depth First Search (DFS) traversal, visiting every node exactly once.

-   **Space Complexity:** $O(H)$
    -   The space complexity is determined by the recursion stack height `H`.
    -   In the worst case (skewed tree), $H = N$.
    -   In the best/average case (balanced tree), $H = \log N$.
