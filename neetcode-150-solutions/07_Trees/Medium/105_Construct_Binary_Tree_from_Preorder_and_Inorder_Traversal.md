# <a href="https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/" target="_blank">105. Construct Binary Tree from Preorder and Inorder Traversal</a>

### Problem Statement
Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

### Approach
To reconstruct the unique binary tree, we leverage the distinct properties of the two traversal orders:
1.  **Preorder Array (`root`, `left`, `right`):** The first element is *always* the root of the current subtree. We use a global pointer (`preIndex`) to pick the next root as we iterate forward.
2.  **Inorder Array (`left`, `root`, `right`):** This array allows us to define the boundaries of the subtrees. Once we know the current root (from preorder), we find its position in the inorder array.
    -   Everything to the **left** of this position belongs to the **left subtree**.
    -   Everything to the **right** of this position belongs to the **right subtree**.



**Algorithm:**
1.  **Map Optimization:** We first store all values from `inorder` into a `HashMap` (`value -> index`). This allows us to find the root's position in $O(1)$ time instead of scanning the array every time.
2.  **Recursion:** We define a recursive function `dfs(left, right)` representing the range in the `inorder` array we are currently building.
    -   **Base Case:** If `left > right`, the range is empty, so return `null`.
    -   **Step 1:** Pick the current root value from `preorder[preIndex]` and increment `preIndex`.
    -   **Step 2:** Create the `TreeNode`.
    -   **Step 3:** Get the `inIndex` (split point) from our Map.
    -   **Step 4:** Recursively build the left subtree using the range `[left, inIndex - 1]`.
    -   **Step 5:** Recursively build the right subtree using the range `[inIndex + 1, right]`.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;

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
    Map<Integer, Integer> indices = new HashMap<>();
    int preIndex = 0;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for(int i = 0 ; i < preorder.length ; i++){
            indices.put(inorder[i], i);
        }

        return dfs(preorder, 0, preorder.length - 1);
    }

    public TreeNode dfs(int[] preorder,int left, int right){
        if(left > right){
            return null;
        }

        TreeNode curr = new TreeNode(preorder[preIndex++]);
        int inIndex = indices.get(curr.val);

        curr.left = dfs(preorder, left, inIndex - 1);
        curr.right = dfs(preorder, inIndex + 1, right);

        return curr;
    }
}
``` 

### Complexity Analysis
Let `N` be the number of nodes in the tree.

-   **Time Complexity:** $O(N)$
    -   We iterate through the `inorder` array once to build the map ($O(N)$).
    -   The `dfs` function is called exactly once for each node to construct it ($O(N)$).
    -   Total time is dominated by $O(N)$.

-   **Space Complexity:** $O(N)$
    -   **HashMap:** Stores `N` entries ($O(N)$).
    -   **Recursion Stack:** In the worst case (a skewed tree), the recursion depth can be $O(N)$. In the average case (balanced tree), it is $O(\log N)$.
