# <a href="https://leetcode.com/problems/serialize-and-deserialize-binary-tree/" target="_blank">297. Serialize and Deserialize Binary Tree</a>

### Problem Statement
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

### Approach
We need a traversal method that allows us to record the tree structure, including `null` children, so we can reconstruct it unambiguously. Your implementation uses **Breadth-First Search (BFS)**

1.  **Serialization (Encode):**
    -   We use a `Queue` to traverse the tree level by level.
    -   We start with the root. For every node we extract from the queue:
        -   If it's **not null**: Append its value to the string (with a comma) and add its children (left and right) to the queue.
        -   If it **is null**: Append `"null,"` to the string.
    -   This produces a comma-separated string representing the tree layer by layer.

2.  **Deserialization (Decode):**
    -   We split the string by comma to get an array of node values.
    -   If the first value is `"null"`, the tree is empty.
    -   We create the `root` from the first value and add it to a `Queue`.
    -   We use an index `i` to iterate through the array values. For every parent node in the queue:
        -   **Left Child:** We check `nodes[i]`. If not `"null"`, create a node, attach it to `parent.left`, and add to queue. Increment `i`.
        -   **Right Child:** We check `nodes[i]`. If not `"null"`, create a node, attach it to `parent.right`, and add to queue. Increment `i`.

### Implementation
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        StringBuilder res = new StringBuilder();

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while(!queue.isEmpty()){
            TreeNode curr = queue.poll();
            if(curr == null){
                res.append("null,");
            }else{
                res.append(curr.val).append(",");
                queue.add(curr.left);
                queue.add(curr.right);
            }
        }

        return res.toString();
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        String[] nodes = data.split(",");
        
        if(nodes[0].equals("null")){
            return null;
        }

        TreeNode root = new TreeNode(Integer.parseInt(nodes[0]));
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int i = 1;

        while(!queue.isEmpty() && i < nodes.length){
            TreeNode curr = queue.poll();

            if(!nodes[i].equals("null")){
                curr.left = new TreeNode(Integer.parseInt(nodes[i]));
                queue.add(curr.left);
            }
            i++;

            if(!nodes[i].equals("null")){
                curr.right = new TreeNode(Integer.parseInt(nodes[i]));
                queue.add(curr.right);
            }
            i++;
        }

        return root;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// TreeNode ans = deser.deserialize(ser.serialize(root));
``` 

### Complexity Analysis
Let `N` be the number of nodes in the tree.

-   **Time Complexity:** $O(N)$
    -   **Serialization:** We visit every node exactly once to build the string.
    -   **Deserialization:** We iterate through the string array (which has size proportional to `N`) exactly once to reconstruct nodes.

-   **Space Complexity:** $O(N)$
    -   **Queue:** In the worst case (a complete binary tree), the last level can contain roughly $N/2$ nodes, so the queue takes $O(N)$.
    -   **String/Array:** The serialized string and the split array store `N` node values (plus nulls), taking $O(N)$ space.
