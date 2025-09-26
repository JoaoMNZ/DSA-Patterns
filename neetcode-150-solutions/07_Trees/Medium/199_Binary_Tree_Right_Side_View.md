# <a href="https://leetcode.com/problems/binary-tree-right-side-view/" target="_blank">199. Binary Tree Right Side View</a>

### Problem Statement
Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return the values of the nodes you can see ordered from top to bottom.

### Approach
This problem can be solved with a clever modification of a **Depth-First Search (DFS)**. The "right side view" is simply the last node encountered at each level. By reversing the standard traversal order, we can ensure this last node is the *first* one we visit at each level.

The strategy is to use a "Root-Right-Left" traversal:
1.  We use a recursive function, passing down the current `level` of the tree.
2.  The key is the order of the recursive calls: we always visit the **right child before the left child**.
3.  We maintain a `result` list. The first time we visit any given level, we know we must be at its right-most node because of our traversal order. We can detect a new level by checking if `result.size() == currentLevel`.
4.  If it's a new level, we add the current node's value to our `result` list. For all other nodes at this same level (which will be to the left), this condition will be false, and their values will be ignored.

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

class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        dfs(result, root, 0);
        return result;
    }

    public void dfs(List<Integer> result, TreeNode currentNode, int currentLevel){
        if (currentNode == null) {
            return;
        }

        if (result.size() == currentLevel) {
            result.add(currentNode.val);
        }

        dfs(result, currentNode.right, currentLevel + 1);
        dfs(result, currentNode.left, currentLevel + 1);
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   The algorithm must visit each node in the tree exactly once.

-   **Space Complexity:** O(n)
    -   The space complexity is determined by the maximum depth of the recursion stack. In the worst case of a skewed tree, this is `O(n)`.
