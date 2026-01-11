# <a href="https://leetcode.com/problems/merge-intervals/" target="_blank">56. Merge Intervals</a>

### Problem Statement
Given an array of `intervals` where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

### Approach
We can solve this using the **Sweep Line algorithm**. We utilize a `TreeMap` (a sorted map) to process the events. For every interval, we add the start point to the map and increment its count, and we add the end point to the map and decrement its count.

When we iterate over the sorted keys of the map, we can track whether we are currently inside an overlapping region using a variable called `have`:

* **Start of an interval:** If `have == 0`, it means no intervals are currently active. We identify this point as the start of a new merged interval and store it in an auxiliary variable.
* **Tracking overlaps:** We add the value of the current point (from the map) to `have`.
* **End of an interval:** If `have` returns to `0` after the update, it means the current chain of overlapping intervals has ended. We treat this point as the end of the merged interval and add the resulting `[start, end]` pair to our result list.

### Implementation
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Map<Integer, Integer> map = new TreeMap<>();
        for(int[] interval : intervals){
            map.put(interval[0], map.getOrDefault(interval[0], 0) + 1);
            map.put(interval[1], map.getOrDefault(interval[1], 0 ) - 1);
        }
        
        List<int[]> res = new ArrayList<>();
        int have = 0;
        int[] aux = new int[2];
        for(int point : map.keySet()){
            if(have == 0){
                aux[0] = point;
            }
            have += map.get(point);
            if(have == 0){
                aux[1] = point;
                res.add(new int[]{aux[0], aux[1]});
            }
        }

        return res.toArray(new int[res.size()][]);
    }
}
```

### Complexity Analysis
Let `N` be the number of intervals in the input array.

-   **Time Complexity:** $O(N \log N)$
    -   We iterate through the intervals to populate the map. Since we are using a `TreeMap`, each insertion takes $O(\log N)$ time. Therefore, building the map takes $O(N \log N)$.
    -   Iterating over the keys of the map takes linear time relative to the number of unique points.

-   **Space Complexity:** $O(N)$
    -   We create a `TreeMap` to store the boundary points. In the worst case (no overlaps), we store $2N+2$ points (start and end for every interval).
    -   The `res` list is used to store the output.
