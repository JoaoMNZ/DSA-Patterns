# <a href="https://leetcode.com/problems/network-delay-time/" target="_blank">743. Network Delay Time</a>

### Problem Statement
You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target.

We send a signal from a given node `k`. Return *the minimum time it takes for all the `n` nodes to receive the signal*. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

### Approach
This problem asks for the minimum time required to reach the *farthest* node from a starting point `k`. Since the edges have non-negative weights (time), this is a classic application of **Dijkstra's Algorithm**.

1.  **Graph Construction:** First, we convert the `times` array into an adjacency list (`graph`) to easily access neighbors and edge weights.
2.  **Dijkstra's Algorithm:**
    -   We use a `PriorityQueue` (Min-Heap) to explore nodes, always picking the one with the shortest accumulated time found so far.
    -   We maintain a `dist` array initialized to `Integer.MAX_VALUE` to track the shortest time to reach each node. The distance to the starting node `k` is set to `0`.
    -   We push the starting node `(k, 0)` into the priority queue.
    -   **Relaxation:** While the queue is not empty, we extract the node with the smallest time. We then look at its neighbors. If we find a path to a neighbor that is faster than the one currently recorded in `dist`, we update the `dist` array and push the neighbor into the queue.
3.  **Result:** After the algorithm finishes, the `dist` array contains the shortest time to reach every node.
    -   If any node still has a distance of `Integer.MAX_VALUE`, it means it's unreachable, so we return `-1`.
    -   Otherwise, the answer is the **maximum** value in the `dist` array (the time it takes for the signal to reach the very last node).

### Implementation
```java
import java.util.*;

class Solution {
    
    class Node implements Comparable<Node> {
        int v;
        int w;

        Node(int v, int w) {
            this.v = v; 
            this.w = w; 
        }

        public int compareTo(Node o){
            return Integer.compare(this.w, o.w);
        }
    }

    public int networkDelayTime(int[][] times, int n, int k) {
        List<Node>[] graph = new ArrayList[n + 1];
        for (int i = 1; i <= n; i++) 
            graph[i] = new ArrayList<>();

        for (int[] edge : times) 
            graph[edge[0]].add(new Node(edge[1], edge[2]));

        int[] dist = new int[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[k] = 0;

        Queue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(k, 0));

        while (!pq.isEmpty()) {
            Node cur = pq.poll();

            if (cur.w > dist[cur.v]) 
                continue;

            for (Node neighbor : graph[cur.v]) {
                int newDist = cur.w + neighbor.w;
                if (newDist < dist[neighbor.v]) {
                    dist[neighbor.v] = newDist;
                    pq.offer(new Node(neighbor.v, newDist));
                }
            }
        }

        int res = 0;
        for (int i = 1; i <= n; i++) {
            if (dist[i] == Integer.MAX_VALUE) 
                return -1;

            res = Math.max(res, dist[i]);
        }
        return res;
    }
}

``` 

### Complexity Analysis
Let `N` be the number of nodes and `E` be the number of edges (length of `times`).

-   **Time Complexity:** O(N + E log N)
    -   Building the graph takes `O(E)`. In Dijkstra's algorithm, each edge is processed once, and operations on the Priority Queue take logarithmic time.

-   **Space Complexity:** O(N + E)
    -   We store the graph in an adjacency list taking `O(N + E)` space. The `dist` array takes `O(N)` space, and the Priority Queue can hold up to `O(E)` nodes in the worst case.
