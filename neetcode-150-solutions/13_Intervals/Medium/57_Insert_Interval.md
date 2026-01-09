# <a href="https://leetcode.com/problems/insert-interval/" target="_blank">57. Insert Interval</a>

### Problem Statement
You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [start_i, end_i]` represent the start and the end of the `i`-th interval and `intervals` is sorted in ascending order by `start_i`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `start_i` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` after the insertion.

### Approach
Since the input `intervals` array is already sorted, we can process the insertion in a single linear pass $O(N)$ by dividing the existing intervals into three distinct groups:

1.  **Non-overlapping (Left):** All intervals that end *before* the `newInterval` starts. These are strictly to the left and can be added to the result immediately.
2.  **Overlapping (Merge):** All intervals that intersect with `newInterval`.
    * We detect overlap when `newInterval` ends after the current interval starts (`newInterval[1] >= intervals[i][0]`).
    * Since we already skipped the "left" ones, we know `intervals[i]` doesn't end before `newInterval` starts.
    * We merge them by updating `newInterval` to encompass the combined range: `min(starts)` and `max(ends)`.
3.  **Non-overlapping (Right):** Once the `newInterval` (now potentially expanded) finishes merging (i.e., it ends before the next interval starts), we add it to the result. Then, we simply add all remaining intervals, as they lie strictly to the right.



### Implementation
```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        int n = intervals.length, i = 0;
        List<int[]> res = new ArrayList<>();

        while(i < n && intervals[i][1] < newInterval[0]){
            res.add(intervals[i]);
            i++;
        }

        while(i < n && newInterval[1] >= intervals[i][0]){
            newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
            i++;
        }

        res.add(newInterval);

        while(i < n){
            res.add(intervals[i]);
            i++;
        }

        return res.toArray(new int[res.size()][]);
        
    }
}
``` 

### Complexity Analysis
Let `N` be the number of intervals in the input array.

-   **Time Complexity:** $O(N)$
    -   We iterate through the `intervals` array exactly once with the index `i`. Each interval is processed and added to the list exactly once.

-   **Space Complexity:** $O(N)$
    -   We create a new list `res` to store the result, which can contain up to `N + 1` intervals.
    -   (If we consider the output space as separate, the auxiliary space complexity is $O(1)$).
