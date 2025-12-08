# <a href="https://leetcode.com/problems/meeting-rooms/" target="_blank">252. Meeting Rooms</a>

### Problem Statement
Given an array of meeting time interval objects consisting of start and end times `[[start_1, end_1], [start_2, end_2], ...]` (where `start_i < end_i`), determine if a person could add all meetings to their schedule without any conflicts.

**Note:** `(0, 8), (8, 10)` is not considered a conflict at 8.

### Approach
To determine if there are any conflicts, we need to check if any two intervals overlap. Comparing every interval with every other interval would be `O(N²)`. We can optimize this by **Sorting**.

1.  **Sort:** We sort the intervals based on their **start times** in ascending order. This aligns the meetings chronologically.

2.  **Scan:** Once sorted, we only need to compare adjacent intervals. If a meeting overlaps with *any* other meeting, it must overlap with the one that starts immediately after it.
3.  **Check Overlap:** We iterate through the sorted intervals. For any two adjacent meetings `i-1` (previous) and `i` (current):
    -   If the **end time** of the previous meeting is **greater than** the **start time** of the current meeting (`i1.end > i2.start`), then there is a conflict. We return `false`.
4.  **Result:** If we finish the loop without finding any overlaps, it means all meetings are compatible. We return `true`.

### Implementation
```java
/**
 * Definition of Interval:
 * public class Interval {
 *     public int start, end;
 *     public Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 * }
 */

import java.util.Collections;
import java.util.Comparator;

class Solution {
    public boolean canAttendMeetings(List<Interval> intervals) {
        Collections.sort(intervals, Comparator.comparing(i -> i.start));
        
        for (int i = 1; i < intervals.size(); i++) {
            Interval i1 = intervals.get(i - 1);
            Interval i2 = intervals.get(i);
            
            if (i1.end > i2.start) {
                return false;
            }
        }

        return true;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of intervals.

-   **Time Complexity:** O(n log n)
    -   The dominant operation is sorting the list of intervals, which takes `O(n log n)`.

-   **Space Complexity:** O(1) or O(n)
    -   The space complexity depends on the sorting implementation.
