# <a href="https://leetcode.com/problems/course-schedule/" target="_blank">207. Course Schedule</a>

### Problem Statement
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]` indicates that to take course `0` you have to first take course `1`.
Return `true` if you can finish all courses. Otherwise, return `false`.

### Approach
This problem is a classic **graph cycle detection** problem. The courses are the nodes (vertices), and the prerequisites are the directed edges. If the graph has a cycle (e.g., Course 0 needs 1, and Course 1 needs 0), it's impossible to finish all courses.

We can find a cycle using **Depth-First Search (DFS)**.
1.  **Build Graph:** First, we build an adjacency list (our `graph`) from the `prerequisites` array to represent the course dependencies.
2.  **DFS Traversal:** We then perform a DFS starting from each course. To detect a cycle, we need to track the state of each node. We use a `visited` array with three states:
    -   `0`: Not yet visited.
    -   `1`: "Visiting" (this node is currently in our active recursion path).
    -   `2`: "Visited" (this node has been fully explored and is part of a safe path).
3.  **Cycle Detection:** During the DFS, if we encounter a node that is already in the "Visiting" (`1`) state, it means we've found a back-edge to a node in our current path, and therefore a cycle exists. If we encounter a "Visited" (`2`) node, we know it's safe and don't need to re-explore it.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for(int i = 0 ; i < numCourses ; i++){
            graph[i] = new ArrayList<>();
        }

        for(int[] edge : prerequisites){
            int u = edge[1];
            int v = edge[0];
            graph[u].add(v);
        }

        int[] visited = new int[numCourses]; 
        for(int i = 0 ; i < numCourses ; i++){
            if(!dfs(graph, visited, i))
                return false;
        }

        return true;
    }

    public boolean dfs(List<Integer>[] graph, int[] visited, int u){
        if(visited[u] == 1)
            return false;
        
        if(visited[u] == 2)
            return true;

        visited[u] = 1;
        for(int v : graph[u]){
            if(!dfs(graph, visited, v)){
                return false;
            }
        }
        visited[u] = 2;

        return true;
    }
}
``` 

### Complexity Analysis
Let `V` be the number of courses (vertices) and `E` be the number of prerequisites (edges).

-   **Time Complexity:** O(V + E)
    -   Building the graph takes `O(E)` time. The DFS traversal visits each vertex (`V`) and each edge (`E`) exactly once.

-   **Space Complexity:** O(V + E)
    -   The `graph` (adjacency list) stores all vertices and edges, taking `O(V + E)` space. The `visited` array takes `O(V)` space, and the recursion stack can go as deep as `V` in the worst case.
