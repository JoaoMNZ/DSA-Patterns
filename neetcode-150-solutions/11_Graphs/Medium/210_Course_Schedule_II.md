# <a href="https://leetcode.com/problems/course-schedule-ii/" target="_blank">210. Course Schedule II</a>

### Problem Statement
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [a_i, b_i]` indicates that you must take course `b_i` first if you want to take course `a_i`.

For example, the pair `[0, 1]` indicates that to take course 0 you have to first take course 1.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

### Approach
This is a classic **Topological Sort** problem. We will use **Kahn's Algorithm** because it naturally generates the order.

The core idea is to start with courses that have no prerequisites (in-degree 0) and proceed by unlocking subsequent courses.

1.  **Build Graph and In-Degrees:** We first construct the adjacency list (the graph) where an edge from $B \to A$ means $B$ is a prerequisite for $A$. Simultaneously, we calculate the **in-degree** for every course, which is the count of its prerequisites.
2.  **Initialize Queue:** We add all courses with an in-degree of 0 (no prerequisites) to a queue. These are the courses we can start with. 
3.  **BFS and Ordering:**
    -   While the queue is not empty, we extract a course $u$.
    -   We add $u$ to our result array (this is the current step in the order).
    -   We look at all neighbors $v$ (courses that $u$ is a prerequisite for).
    -   We decrement the in-degree of $v$, effectively removing $u$'s dependency.
    -   If the in-degree of $v$ becomes 0, it means all of $v$'s prerequisites are now met, and we add $v$ to the queue.
4.  **Cycle Check:** If the length of the final order equals `numCourses`, then a valid ordering exists, and we return the result. If the length is less than `numCourses`, it means a cycle exists (a set of courses that depend on each other), making a complete schedule impossible.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;
import java.util.Deque;
import java.util.ArrayDeque;

class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for(int i = 0 ; i < numCourses ; i++){
            graph[i] = new ArrayList<>();
        }

        int[] indegree = new int[numCourses];
        for(int[] prerequisite : prerequisites){
            int u = prerequisite[1];
            int v = prerequisite[0];
            graph[u].add(v);
            indegree[v]++;
        }

        Deque<Integer> queue = new ArrayDeque<>();
        for(int i = 0 ; i < numCourses ; i++){
            if(indegree[i] == 0){
                queue.addLast(i);
            }
        }

        int[] res = new int[numCourses];
        int order = 0;

        while(!queue.isEmpty()){
            int u = queue.poll();
            res[order++] = u; 

            for(int v : graph[u]){
                indegree[v]--;
                if(indegree[v] == 0){
                    queue.addLast(v);
                }
            }
        }

        return order == numCourses ? res : new int[0];
    }
}
``` 

### Complexity Analysis
Let `V` be the number of courses (`numCourses`) and `E` be the number of prerequisites.

-   **Time Complexity:** O(V + E)
    -   We iterate through all edges once to build the graph and calculate in-degrees (`O(E)`). The BFS visits every node and every edge exactly once

-   **Space Complexity:** O(V + E)
    -   The adjacency list stores the graph structure (`O(V + E)`). The `indegree` array and the queue store up to `O(V)` elements.
