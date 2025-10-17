# <a href="https://leetcode.com/problems/clone-graph/" target="_blank">133. Clone Graph</a>

### Problem Statement
Given a reference of a node in a **connected undirected graph**. Return a **deep copy** (clone) of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.

### Approach
The main challenge in a deep copy problem, especially with graphs, is to handle cycles and ensure that each node is only copied once. A **`HashMap`** is the perfect tool to solve this, acting as both a "visited" set and a map from an original node to its new copy.

The algorithm uses a **Depth-First Search (DFS)** to traverse the graph:
1.  **Base Case:** If the node we are trying to clone is `null`, we return `null`. If the node is already present in our map, it means we've already created its copy, so we simply return the copy from the map. This step is crucial for preventing infinite loops in graphs with cycles.
2.  **Recursive Step:** If the node is not yet in our map:
    -   First, create a new `clonedNode` with the same value.
    -   Immediately put this new `clonedNode` into the map, associating it with the `originalNode`. This must be done *before* traversing its neighbors.
    -   Then, iterate through each `neighbor` of the `originalNode`. For each one, make a recursive call to our `dfs` function. The node returned by that call will be the correct cloned neighbor, which we add to our `clonedNode`'s neighbors list.

This process ensures that every node is created only once and all connections are correctly re-established in the new graph.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;

/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

import java.util.Map;
import java.util.HashMap;

class Solution {
    public Node cloneGraph(Node node) {
        Map<Node, Node> oldToNew = new HashMap<>();
        return dfs(node, oldToNew);
    }

    private Node dfs(Node originalNode, Map<Node, Node> oldToNew){
        if (originalNode == null) {
            return null;
        }

        if(oldToNew.containsKey(originalNode)){
            return oldToNew.get(originalNode);
        }

        Node clonedNode = new Node(originalNode.val);
        oldToNew.put(originalNode, clonedNode);

        for(Node neighbor : originalNode.neighbors){
            clonedNode.neighbors.add(dfs(neighbor, oldToNew));
        }

        return clonedNode;
    }
}
``` 

### Complexity Analysis
Let `V` be the number of nodes (vertices) and `E` be the number of edges in the graph.

-   **Time Complexity:** O(V + E)
    -   The algorithm traverses each node and each edge exactly once during the DFS.

-   **Space Complexity:** O(V)
    -   The space is determined by the `HashMap` which stores all `V` nodes, as well as the depth of the recursion stack, which can be up to `V` in the worst case.
