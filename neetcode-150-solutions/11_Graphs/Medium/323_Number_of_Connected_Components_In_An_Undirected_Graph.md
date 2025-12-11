# <a href="https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/" target="_blank">323. Number of Connected Components in an Undirected Graph</a>

### Problem Statement
There is an undirected graph with `n` nodes. There is also an `edges` array, where `edges[i] = [a, b]` means that there is an edge between node `a` and node `b` in the graph.

The nodes are numbered from `0` to `n - 1`.

Return the total number of connected components in that graph.

### Approach
This problem is structurally identical to "Number of Islands." A connected component is simply a set of nodes that can reach each other. We can find them using a **Depth-First Search (DFS)**.

1.  **Build the Graph:** Since we are given a list of edges, we first construct an adjacency list. Because the graph is undirected, for every edge `[a, b]`, we verify that `a` points to `b` and `b` points to `a`.
2.  **Iterate and Count:** We iterate through each node from `0` to `n - 1`. We maintain a `visited` array to keep track of nodes we have already encountered.
3.  **DFS Traversal:** If we encounter a node that has **not** been visited yet:
    -   It means we have found a new, unexplored component.
    -   We increment our component counter (`res`).
    -   We immediately start a DFS from this node to visit (and mark) every other node connected to it. This ensures we don't count the same component twice.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    boolean[] visited;

    public int countComponents(int n, int[][] edges) {
        List<Integer>[] graph = new ArrayList[n];
        for(int i = 0 ; i < n ; i++){
            graph[i] = new ArrayList<>();
        }

        for(int[] edge : edges){
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }

        visited = new boolean[n];

        int res = 0;
        for(int i = 0 ; i < n ; i++){
            if(!visited[i]){
                dfs(graph, i);
                res++;
            }
        }
        return res;
    }

    public void dfs(List<Integer>[] graph, int u){
        visited[u] = true;
        
        for(int v : graph[u]){
            if(!visited[v]){
                dfs(graph, v);
            }
        }
    }
}
``` 

### Complexity Analysis
Let `V` be the number of nodes (`n`) and `E` be the number of edges.

-   **Time Complexity:** O(V + E)
    -   Building the adjacency list takes `O(E)`. The DFS traversal visits every node and every edge exactly once, taking `O(V + E)`.

-   **Space Complexity:** O(V + E)
    -   The adjacency list stores all nodes and edges, taking `O(V + E)`. The `visited` array takes `O(V)`, and the recursion stack can go up to `O(V)` in the worst case.
