# <a href="https://leetcode.com/problems/redundant-connection/" target="_blank">684. Redundant Connection</a>

### Problem Statement
In this problem, a tree is an undirected graph that is connected and has no cycles.

You are given a graph that started as a tree with `n` nodes labeled from `1` to `n`, with one additional edge added. The added edge has two different vertices chosen from `1` to `n`, and was not an edge that already existed. The graph is represented as an array `edges` of length `n` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the graph.

Return an edge that can be removed so that the resulting graph is a tree of `n` nodes. If there are multiple answers, return the answer that occurs last in the input.

### Approach
We can solve this efficiently using the **Union-Find** data structure.

A tree with `n` nodes must have exactly `n - 1` edges. The problem statement tells us there are `n` edges, meaning exactly one edge creates a cycle. Our goal is to find that specific edge.



1.  **Initialization:** We treat each node as its own separate set initially.
2.  **Iterate Edges:** We process the edges one by one in the order they appear in the input.
3.  **Union-Find Logic:** For each edge `(u, v)`:
    -   We find the representative of `u`.
    -   We find representative of `v`.
    -   **Detection:** If `root(u) == root(v)`, it means `u` and `v` are **already connected** through some other path. Adding this current edge `(u, v)` would close a loop and create a cycle. Since we need to return the *last* edge that creates a cycle, and we are iterating in order, this is our answer.
    -   **Merge:** If they have different roots, we **union** the two sets (connect them) and continue.

We use **Path Compression** in the `find` function and **Union by Rank** to keep the tree flat, ensuring operations are nearly constant time.

### Implementation
```java
class Solution {
    int[] parent;
    int[] rank;

    public int[] findRedundantConnection(int[][] edges) {
        int n = edges.length;
        parent = new int[n + 1];
        rank = new int[n + 1];
        for(int i = 1 ; i <= n ; i++){
            parent[i] = i;
            rank[i] = 1;
        }

        for(int[] edge : edges){
            if(!union(edge[0], edge[1])){
                return edge;
            }
        }

        return new int[0];
    }

    public boolean union(int node1, int node2){
        int representative1 = find(node1);
        int representative2 = find(node2);

        if(representative1 == representative2){
            return false;
        }

        if(rank[representative1] > rank[representative2]){
            parent[representative2] = representative1;
            rank[representative1] += rank[representative2];
        }else{
            parent[representative1] = representative2;
            rank[representative2] += rank[representative1];
        }

        return true;
    }

    public int find(int node){
        if(node != parent[node]){
            parent[node] = find(parent[node]);
        }
        
        return parent[node];
    }
}
``` 

### Complexity Analysis
Let `N` be the number of nodes (which is equal to the number of edges in this problem).

-   **Time Complexity:** O(N * α(N)) ≈ O(N)
    -   We iterate through `N` edges. For each edge, we perform `union` and `find` operations.
    -   Thanks to **Path Compression** and **Union by Rank**, the amortized time complexity of each operation is `O(α(N))`, where `α` is the Inverse Ackermann function. For all practical values of `N`, `α(N)` is $\le 4$, making it effectively constant time. Thus, the total time is nearly linear `O(N)`.

-   **Space Complexity:** O(N)
    -   We use `parent` and `rank` arrays of size `N + 1` to store the DSU state.
