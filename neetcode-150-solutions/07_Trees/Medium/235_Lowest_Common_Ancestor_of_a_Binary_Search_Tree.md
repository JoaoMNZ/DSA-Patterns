# <a href="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/" target="_blank">235. Lowest Common Ancestor of a Binary Search Tree</a>

### Problem Statement
Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in the tree that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself).”

### Approach
The key to solving this problem efficiently is to leverage the properties of a **Binary Search Tree (BST)**. In a BST, all nodes with values smaller than the root are in the left subtree, and all nodes with values larger are in the right subtree.

We can use this property to find the Lowest Common Ancestor (LCA) by starting at the root and making a simple decision at each node:

1.  If the values of both `p` and `q` are **greater than** the current node's value, it means the LCA must be in the **right subtree**. We can safely ignore the left subtree and continue our search from the right child.
2.  If the values of both `p` and `q` are **less than** the current node's value, the LCA must be in the **left subtree**. We can ignore the right subtree and continue our search from the left child.
3.  If neither of the above is true, it means we have found the "split point" where `p` and `q` are on different sides of the current node (or one of them is the current node). This means the current node is the lowest node that is an ancestor to both, so it must be the LCA.

### Implementation
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 * int val;
 * TreeNode left;
 * TreeNode right;
 * TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (p.val > root.val && q.val > root.val) {
            return lowestCommonAncestor(root.right, p, q);
        } else if (p.val < root.val && q.val < root.val) {
            return lowestCommonAncestor(root.left, p, q);
        } else {
            return root;
        }
    }
}
``` 

### Complexity Analysis
Let `h` be the height of the tree.

-   **Time Complexity:** O(h)
    -   The algorithm traverses the tree from the root down to the LCA node. In the worst case, this path is equal to the height of the tree.

-   **Space Complexity:** O(h)
    -   The space required is for the recursion call stack. The maximum depth of the stack will be the height of the tree, `h`.
