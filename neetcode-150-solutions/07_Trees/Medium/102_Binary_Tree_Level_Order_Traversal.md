# <a href="https://leetcode.com/problems/binary-tree-level-order-traversal/" target="_blank">102. Binary Tree Level Order Traversal</a>

### Problem Statement
Given the `root` of a binary tree, return the *level order traversal* of its nodes' values. (i.e., from left to right, level by level).

### Approach
Although the result is ordered by level (suggesting a Breadth-First Search), this problem can be solved cleanly with a recursive **Depth-First Search (DFS)** approach. The key is to pass down the current `level` as a parameter during the recursion.

1.  We maintain a main `result` list, which will contain a sublist for each level of the tree.
2.  Our recursive `dfs` function takes the current node and its `level` as arguments.
3.  The first time we visit a node at a new `level`, our `result` list won't have a sublist ready for it. We can easily detect this by checking if `result.size() == level`. If this is true, we add a new empty list to `result`.
4.  We then add the current node's value to the sublist that corresponds to its `level`.
5.  Finally, we make the recursive calls for the left and right children, passing `level + 1` to them.

This process ensures that nodes are always added to the correct level list as the tree is explored.

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

import java.util.List;
import java.util.ArrayList;
import java.util.LinkedList;

class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        dfs(result, root, 0);
        return result;
    }

    public void dfs(List<List<Integer>> result, TreeNode currentNode, int level){
        if(currentNode == null){
            return;
        }

        if(result.size() == level){
            result.add(new LinkedList<>());
        }

        result.get(level).add(currentNode.val);
        dfs(result, currentNode.left, level + 1);
        dfs(result, currentNode.right, level + 1);
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   The algorithm must visit each node in the tree exactly once.

-   **Space Complexity:** O(n)
    -   The space complexity is determined by the recursion stack.
