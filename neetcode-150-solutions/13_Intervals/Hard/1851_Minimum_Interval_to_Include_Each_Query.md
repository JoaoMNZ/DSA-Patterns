# <a href="https://leetcode.com/problems/minimum-interval-to-include-each-query/" target="_blank">1851. Minimum Interval to Include Each Query</a>

### Problem Statement
You are given a 2D integer array `intervals`, where `intervals[i] = [left_i, right_i]` describes the $i$-th interval starting at `left_i` and ending at `right_i` (inclusive). The **size** of an interval is defined as the number of integers it contains, or more formally `right_i - left_i + 1`.

You are also given an integer array `queries`. The answer to the $j$-th query is the **size of the smallest interval** `i` such that `left_i <= queries[j] <= right_i`. If no such interval exists, the answer is `-1`.

Return an array containing the answers to the queries.

### Approach
To solve this problem efficiently, we use an **Offline Query Processing** strategy combined with a **Min-Heap (Priority Queue)**. Since we need to find the *smallest* interval for each query, a Min-Heap is ideal. Processing queries in sorted order allows us to efficiently manage which intervals are currently "active".

1.  **Sorting:**
    * **Queries:** We sort the queries in ascending order. Since the output must correspond to the original order of queries, we store the original indices before sorting.
    * **Intervals:** We sort the intervals by their start time. This allows us to process intervals sequentially as the query value increases.

2.  **Processing (Sweep Line):**
    * We iterate through the sorted queries.
    * **Add Intervals:** For the current query, we add all intervals that have started (`interval[0] <= query`) to the Min-Heap. We store the pair `[size, end_time]` in the heap, ordered by size.
    * **Remove Invalid Intervals:** We check the top of the heap. If the interval with the smallest size ends *before* the current query (`heap.peek()[1] < query`), it can no longer cover this query (or any future queries, since they are sorted). We remove it (`poll`). We repeat this until the top of the heap is valid or the heap is empty.
    * **Record Answer:** If the heap is not empty, the top element represents the smallest valid interval covering the current query. Otherwise, the result is `-1`.

### Implementation
```java
class Solution {
    public int[] minInterval(int[][] intervals, int[] queries) {
        PriorityQueue<int[]> minHeap = new PriorityQueue<>(Comparator.comparing(n -> n[0]));

        Integer[] indexes = new Integer[queries.length];
        for (int j = 0; j < queries.length; j++) {
            indexes[j] = j;
        }
        Arrays.sort(indexes, (a,b) -> Integer.compare(queries[a], queries[b]));

        Arrays.sort(intervals, (a,b) -> Integer.compare(a[0], b[0]));
        int[] res = new int[queries.length];
        int i = 0;

        for(int index : indexes){
            int query = queries[index];
            while(i < intervals.length && intervals[i][0] <= query){
                int l = intervals[i][0];
                int r = intervals[i][1];
                minHeap.offer(new int[]{r - l + 1, r});
                i++;
            }

            while(!minHeap.isEmpty() && minHeap.peek()[1] < query){
                minHeap.poll();
            }
            
            res[index] = minHeap.isEmpty() ? -1 : minHeap.peek()[0];
        }

        return res;
    }
}
```

### Complexity Analysis
Let `N` be the number of intervals and `M` be the number of queries.

-   **Time Complexity:** $O(N \log N + M \log M)$
    -   Sorting the `intervals` array takes $O(N \log N)$.
    -   Sorting the `queries` (via the index array) takes $O(M \log M)$.
    -   During the main loop, each interval is pushed into and popped from the priority queue at most once. Heap operations take $O(\log N)$. This results in a total of $O(N \log N)$ for heap operations.
    -   We also iterate through the sorted queries once.
    -   Combining these steps, the overall time complexity is dominated by the sorting of intervals and queries: $O(N \log N + M \log M)$.

-   **Space Complexity:** $O(N + M)$
    -   **Heap:** In the worst case, the priority queue stores all `N` intervals, requiring $O(N)$ space.
    -   **Auxiliary Arrays:** We use an `indexes` array of size `M` to sort the queries while keeping track of their original positions.
    -   The output array `res` takes $O(M)$ space.
