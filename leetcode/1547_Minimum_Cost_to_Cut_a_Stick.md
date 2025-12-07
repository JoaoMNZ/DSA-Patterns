# <a href="https://leetcode.com/problems/minimum-cost-to-cut-a-stick/" target="_blank">1547. Minimum Cost to Cut a Stick</a>

### Problem Statement
Given a wooden stick of length `n` units. The stick is labeled from `0` to `n`. You are given an integer array `cuts` where `cuts[i]` denotes a position you should perform a cut at.

You should perform the cuts in order, you can change the order of the cuts as you wish.
The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut).

Return *the minimum total cost of the cuts*.

### Approach
This is a classic **Interval DP** problem. We need to find the optimal order of cuts to minimize the cost.

We can think of this recursively: for a given segment of the stick defined by `start` and `end`, if we choose to make a cut at position `k` (where `start < k < end`), the cost becomes:
`Cost = (end - start) + dfs(start, k) + dfs(k, end)`

Since there are many overlapping subproblems (the same segment `(start, end)` might be reached through different cutting orders), we use **Memoization**.

1.  **State:** The state is defined by the `start` and `end` coordinates of the current stick segment.
2.  **Recursive Step:** We iterate through all available `cuts`. If a cut falls strictly inside our current segment (`start < cut < end`), we calculate the cost of making that cut first and then solving for the left and right pieces recursively. We keep track of the minimum cost among all valid cuts.
3.  **Base Case:** If there are no valid cuts between `start` and `end`, the cost is `0`.
4.  **Memoization:** We use a `Map` to store the computed result for each `start, end` pair to avoid re-calculation.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;

public class Solution {
    Map<String, Integer> cache = new HashMap<>();

    public int minCost(int n, int[] cuts) {
        return dfs(cuts, 0, n);
    }

    private int dfs(int[] cuts, int start, int end) {
        if(cache.containsKey(start + ", " + end))
            return cache.get(start + ", " + end);

        int minCost = Integer.MAX_VALUE;
        for(int cut : cuts){
            if(cut <= start || cut >= end)
                continue;
            
            int left = dfs(cuts, start, cut);
            int right = dfs(cuts, cut, end);
            minCost = Math.min(minCost, (end - start) + left + right);
        }

        cache.put(start + ", " + end, minCost == Integer.MAX_VALUE ? 0 : minCost);
        return cache.get(start + ", " + end);
    }
}
``` 

### Complexity Analysis
Let `m` be the length of the `cuts` array and `N=min(n,m)`

-   **Time Complexity:** O(N² * m)
    -   The number of relevant coordinate points is `N`. The number of distinct states `(start, end)` in our recursion is proportional to `N²`. Inside each recursive call, we iterate through the `cuts` array again, which takes `O(m)`.

-   **Space Complexity:** O(N²)
    -   The `cache` map stores results for unique segment pairs. Since there are roughly `N` possible cut points, there are `O(N²)` possible segments.
