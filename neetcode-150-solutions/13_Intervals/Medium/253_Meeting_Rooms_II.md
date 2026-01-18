# <a href="https://leetcode.com/problems/meeting-rooms-ii/" target="_blank">253. Meeting Rooms II</a>

### Problem Statement
Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of conference rooms required.

### Approach
To solve this problem efficiently, we use a **Line Sweep** algorithm combined with a `TreeMap`. We treat the timeline as a series of events: starting a meeting increases room demand, and ending a meeting decreases it.

1.  **Data Structure:** We use a `TreeMap<Integer, Integer>`.
    * **Key:** The time point (start or end of a meeting).
    * **Value:** The net change in the number of rooms needed at that specific time.

2.  **Algorithm:**
    * Iterate through all intervals. For each `start` time, increment the count (`+1`). For each `end` time, decrement the count (`-1`).
    * Traverse the `TreeMap` keys. Maintain a running sum (representing the number of active meetings at that moment).
    * The answer is the maximum value this running sum reaches at any point during the traversal.

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

class Solution {
    public int minMeetingRooms(List<Interval> intervals) {
        Map<Integer, Integer> map = new TreeMap<>();
        for(Interval interval : intervals){
            map.put(interval.start, map.getOrDefault(interval.start, 0) + 1);
            map.put(interval.end, map.getOrDefault(interval.end, 0) - 1);
        }

        int res = 0;
        int days = 0;
        for(int point : map.keySet()){
            days += map.get(point);
            res = Math.max(res, days);
        }

        return res;
    }
}
```

### Complexity Analysis
Let `N` be the number of intervals in the input list.

-   **Time Complexity:** $O(N \log N)$
    -   We iterate through the intervals and perform two insertions into the `TreeMap` for each interval (one for the start time and one for the end time).
    -   Since a `TreeMap` is implemented as a Red-Black Tree (a balanced binary search tree), each insertion operation takes $O(\log N)$ time.

-   **Space Complexity:** $O(N)$
    -   The `TreeMap` stores the unique start and end times from the input.
