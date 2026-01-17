# <a href="https://leetcode.com/problems/non-overlapping-intervals/" target="_blank">435. Non-overlapping Intervals</a>

### Problem Statement
Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

Note that intervals which only touch at a point are non-overlapping. For example, `[1, 2]` and `[2, 3]` are non-overlapping.

### Approach
We approach this problem using a **Greedy Strategy**. The goal is to minimize the number of removals, which is equivalent to maximizing the number of non-overlapping intervals we can keep.

1.  **Sorting:**
    We sort the intervals by their **end times** in ascending order.
    * *Reasoning:* To fit as many intervals as possible, we should always pick the interval that finishes earliest. This leaves the maximum amount of "free time" remaining for subsequent intervals.

2.  **Iteration:**
    We iterate through the sorted intervals and maintain a variable `prevEnd` to track the end time of the last valid (non-removed) interval.
    * **Overlap Detected (`interval[0] < prevEnd`):** If the current interval starts before the previous one ends, they overlap. Since the list is sorted by end time, the current interval ends *after* (or at the same time as) the `prevEnd`. To be greedy, we remove the current interval (increment the removal count) because it occupies more time/space. We **do not** update `prevEnd`.
    * **No Overlap (`interval[0] >= prevEnd`):** The intervals do not conflict. We "keep" this interval and update `prevEnd` to the end of the current interval (`interval[1]`).

### Implementation
```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a,b) -> Integer.compare(a[1], b[1]));
        int prevEnd = Integer.MIN_VALUE;
        int res = 0;

        for(int[] interval : intervals){
            if(prevEnd > interval[0]){
                res++;
            }else{
                prevEnd = interval[1];
            }
        }

        return res;
    }
}
```

### Complexity Analysis
Let `N` be the number of intervals in the input array.

-   **Time Complexity:** $O(N \log N)$
    -   Sorting the `intervals` array takes $O(N \log N)$ time.
    -   Iterating through the sorted array takes linear time $O(N)$.
    -   Therefore, the overall time complexity is dominated by the sorting step.

-   **Space Complexity:** $O(\log N)$ or $O(N)$
    -   We use $O(1)$ extra space for variables `prevEnd` and `res`.
    -   However, the sorting algorithm (`Arrays.sort` in Java) typically requires $O(\log N)$ stack space (for QuickSort variants) or $O(N)$ auxiliary space (for MergeSort/TimSort variants used with object arrays like `int[][]`).
