# <a href="https://leetcode.com/problems/graph-valid-tree/" target="_blank">261. Graph Valid Tree</a>

### Problem Statement
Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

### Approach
For an undirected graph to be a **valid tree**, it must satisfy two conditions:
1.  **Fully Connected:** Starting from any node (e.g., node 0), you must be able to reach every other node in the graph. 
2.  **No Cycles:** There must be no loops in the graph. 

We can verify both conditions using **Breadth-First Search (BFS)**:
1.  **Build Graph:** Construct an adjacency list from the `edges`.
2.  **BFS Traversal:** Start BFS from node `0`. We use a `Set` to track visited nodes and a `Queue` to manage the traversal.
    -   To detect cycles in an undirected graph, we must keep track of the `parent` of the current node (the node we came from).
    -   When looking at neighbors of the current node `u`, if we encounter a neighbor `v` that has **already been visited** AND `v` is **not** the `parent` of `u`, then we have found a "back edge," which implies a cycle. We return `false`.
3.  **Connectivity Check:** After the BFS completes, if the size of our `visited` set equals `n`, it means we reached all nodes. If `visited.size() < n`, the graph is disconnected.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;
import java.util.Deque;
import java.util.ArrayDeque;
import java.util.Set;
import java.util.HashSet;

class Solution {
    public boolean validTree(int n, int[][] edges) {
        List<Integer>[] graph = new ArrayList[n];
        for(int i = 0 ; i < n ; i++){
            graph[i] = new ArrayList<>();
        }

        for(int[] edge : edges){
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }

        Set<Integer> visited = new HashSet<>();
        Deque<int[]> queue = new ArrayDeque<>();
        queue.addLast(new int[]{0, -1});
        
        while(!queue.isEmpty()){
            int[] pair = queue.poll();
            int u = pair[0];
            int parent = pair[1];
            visited.add(u);

            for(int v : graph[u]){
                if(v == parent)
                    continue;
                
                if(!visited.add(v))
                    return false;
                
                queue.addLast(new int[]{v, u});
            }
        }

        return visited.size() == n ? true : false;
    }
}
``` 

### Complexity Analysis
Let `V` be the number of nodes (`n`) and `E` be the number of edges.

-   **Time Complexity:** O(V + E)
    -   We build the adjacency list in `O(E)`. In the BFS, we visit each node and edge at most once, which takes `O(V + E)`.

-   **Space Complexity:** O(V + E)
    -   The adjacency list requires `O(V + E)` space. The `visited` set and the queue can store up to `O(V)` nodes.
