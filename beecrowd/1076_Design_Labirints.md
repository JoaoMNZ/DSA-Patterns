# <a href="https://judge.beecrowd.com/en/problems/view/1076" target="_blank">1076. Design Labirints</a>

### Problem Statement
Given a series of test cases, each test case provides a starting node `N`, a number of vertices `V`, and a number of edges `A`. The edges represent a connected, undirected graph. We must find the total number of movements required to traverse every reachable edge from `N` exactly twice (once in each direction).

### Approach
The problem asks for the total number of movements, which simplifies to finding the number of *unique edges* in the connected component of the starting node `N` and multiplying that count by 2.

A **Depth-First Search (DFS)** is a straightforward way to explore this connected component. The main challenge is to count each *edge* only once, even in an undirected graph where an edge `(u, v)` is the same as `(v, u)`.

1.  We use a `visited` set to store the edges we have already traversed.
2.  To handle the undirected nature, we store a canonical representation of each edge. A `tuple` sorted by node value (e.g., `(2, 5)` for an edge between 2 and 5, regardless of direction) is a perfect key.
3.  We start the DFS from the given node `N`.
4.  During the traversal, for each neighbor `v` of the current node `u`, we create the sorted edge tuple.
5.  If this edge is not in our `visited` set, we add it and recursively call DFS on the neighbor `v`.
6.  The DFS will explore all reachable nodes and, in the process, add all *unique reachable edges* to the `visited` set.
7.  The function returns the final size of the `visited` set (the total unique edges), which we then multiply by 2 for the final answer.

### Implementation
```python
import sys
from collections import defaultdict

sys.setrecursionlimit(2000)

input = sys.stdin.readline
print = sys.stdout.write
def println(text):
    print(str(text) + '\n')

T = int(input())
for _ in range(T):
    def dfs(u):
        for v in graph[u]:
            edge = tuple(sorted((u,v)))
            if edge not in visited:
                visited.add(edge)
                dfs(v)
                
        return len(visited)
        
    N = int(input())
    V, A = map(int, input().split())
    
    graph = defaultdict(list)
    visited = set()
    
    for _ in range(A):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)
    
    println(dfs(N) * 2)
``` 

### Complexity Analysis
Let `V` be the number of vertices and `A` be the number of edges.

-   **Time Complexity:** O(V + A)
    -   This analysis is per test case. We build the graph by reading all `A` edges. The DFS, in the worst case, visits every reachable vertex `V` and every reachable edge `A` in the component exactly once.

-   **Space Complexity:** O(V + A)
    -   The `graph` (adjacency list) stores all vertices and edges, taking `O(V + A)` space. The `visited` set stores at most `A` edges. The recursion stack for DFS can go as deep as `V` in the worst case.
