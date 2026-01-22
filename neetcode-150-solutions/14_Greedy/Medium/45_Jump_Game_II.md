# <a href="https://leetcode.com/problems/jump-game-ii/" target="_blank">45. Jump Game II</a>

### Problem Statement
You are given a 0-indexed array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:
* `0 <= j <= nums[i]` and
* `i + j < n`

Return the minimum number of jumps to reach `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

### Approach
We can solve this using a **Greedy BFS** strategy. We process the array in "levels" of indices reachable with the same number of jumps.

1.  **Level Traversal:** The variable `i` tracks our current position, and `temp` marks the end of the range reachable with the current number of `jumps`.
2.  **Finding the Next Max Reach:** Within the current level (from `i` to `temp`), we iterate to find the farthest index we can reach (`maxReached`) in the *next* jump.
3.  **Making the Jump:** Once `i` exceeds `temp`, it means we have finished processing all possibilities for the current jump count. We increment `jumps` and update our range limit (`temp`) to the `maxReached` we calculated.

### Implementation
```java
class Solution {
    public int jump(int[] nums) {
        int jumps = 0;
        int maxReached = 0;
        int i = 0;
        while(maxReached < nums.length - 1){
            int temp = maxReached;
            while(i <= temp){
                maxReached = Math.max(maxReached, i + nums[i]);
                i++;
            }
            jumps++;
        }
        return jumps;
    }
}
```

### Complexity Analysis
Let `N` be the length of the `nums` array.

-   **Time Complexity:** $O(N)$
    -   We iterate through the array using the variable `i`. Even though there are nested `while` loops, `i` starts at 0 and is incremented linearly up to `N`.
    -   Each element in the array is visited and processed exactly once.

-   **Space Complexity:** $O(1)$
    -   The algorithm uses a constant amount of extra space for variables `jumps`, `maxReached`, `i`, and `temp`.
    -   No additional data structures are used relative to the input size.
