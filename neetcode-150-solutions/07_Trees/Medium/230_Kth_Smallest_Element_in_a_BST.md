# <a href="https://leetcode.com/problems/kth-smallest-element-in-a-bst/" target="_blank">230. Kth Smallest Element in a BST</a>

### Problem Statement
Given the `root` of a binary search tree, and an integer `k`, return the `k`th smallest value (**1-indexed**) of all the values of the nodes in the tree.

### Approach
A key property of a **Binary Search Tree (BST)** is that an **Inorder Traversal** (Left -> Root -> Right) visits the nodes in strictly increasing order.

We can leverage this property directly:
1.  Perform a recursive Inorder DFS traversal.
2.  Store the value of each node visited in a list.
3.  Since the list is now sorted from smallest to largest, the `k`th smallest element is simply the element at index `k - 1` in the list.

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

import java.util.List;
import java.util.ArrayList;

public class Solution {
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> arr = new ArrayList<>();
        dfs(root, arr);
        return arr.get(k - 1);
    }

    private void dfs(TreeNode node, List<Integer> arr) {
        if (node == null) {
            return;
        }

        dfs(node.left, arr);
        arr.add(node.val);
        dfs(node.right, arr);
    }
}
``` 

### Complexity Analysis
Let `n` be the number of nodes in the tree.

-   **Time Complexity:** O(n)
    -   The algorithm visits every node in the tree exactly once to build the sorted list.

-   **Space Complexity:** O(n)
    -   We use an `ArrayList` to store all `n` node values. Additionally, the recursion stack takes `O(n)` space in the worst case.
