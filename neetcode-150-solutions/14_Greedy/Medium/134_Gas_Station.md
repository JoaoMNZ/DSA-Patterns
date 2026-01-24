# <a href="https://leetcode.com/problems/gas-station/" target="_blank">134. Gas Station</a>

### Problem Statement
There are `n` gas stations along a circular route, where the amount of gas at the $i$-th station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the $i$-th station to its next $(i + 1)$-th station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return `-1`. If there exists a solution, it is guaranteed to be unique.

### Approach
We can solve this using a **Greedy Strategy**.

1.  **Feasibility Check:** First, we check if the total amount of gas available is greater than or equal to the total cost required. If `sum(gas) < sum(cost)`, it is mathematically impossible to complete the circuit, so we return `-1`.

2.  **Finding the Start Point:** If the circuit is possible, we need to find the valid starting index. We iterate through the stations, tracking the current fuel in the `tank`:
    * We add the net gain (`gas[i] - cost[i]`) to the `tank`.
    * If `tank` becomes negative at any point, it means we cannot reach station `i+1` starting from our current start point (or any point between the current start and `i`).
    * Therefore, we reset the `tank` to 0 and tentatively set the *next* station (`i + 1`) as the new starting point (`res`).

### Implementation
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        if (Arrays.stream(gas).sum() < Arrays.stream(cost).sum()) {
            return -1;
        }

        int res = 0;
        int tank = 0;

        for(int i = 0 ; i < gas.length ; i++){
            tank += (gas[i] - cost[i]);
            if(tank < 0){
                tank = 0;
                res = i + 1;
            }
        }
        
        return res;
    }
}
```

Here is the Complexity Analysis block separately:

### Complexity Analysis
Let `N` be the number of gas stations.

-   **Time Complexity:** $O(N)$
    -   We perform two linear passes (or one, depending on implementation details): one pass to check if the total gas is sufficient (using `Arrays.stream`), and one pass to find the starting index.
    -   Both operations are linear, resulting in $O(N)$.

-   **Space Complexity:** $O(1)$
    -   The algorithm uses a fixed number of integer variables (`res`, `tank`) regardless of the input size.
