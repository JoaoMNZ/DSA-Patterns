# <a href="https://leetcode.com/problems/jump-game/" target="_blank">55. Jump Game</a>

### Problem Statement
You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

### Approach
This problem can be solved with a **Greedy Algorithm**. We don't need to know the exact path; we only need to know the furthest possible index we can reach at any given moment.

The key is to use a single variable, let's call it `farthest`, to keep track of the maximum index we can jump to.

1.  We initialize `farthest` to `0`.
2.  We iterate through the array with an index `i`.
3.  In each step, we first check if our current position `i` is reachable. If `i` is greater than `farthest`, it means we got stuck at a previous position and can never reach `i`. In this case, we return `false`.
4.  If we can reach `i`, we calculate how far we can jump from this position (`i + nums[i]`) and update `farthest` if this new reach is greater than our current `farthest`.
5.  If the loop finishes, it means we were able to reach every position up to the end (or had the potential to jump past it), so we return `true`.

### Implementation
```java
class Solution {
    public boolean canJump(int[] nums) {
        int farthest = 0;
        for(int i = 0 ; i < nums.length ; i++){
            if(i > farthest){
                return false;
            }
            int jump = i + nums[i];
            farthest = Math.max(farthest, jump);
        }
        return true;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n)
  - We perform a single pass through the array of `n` elements, making the time complexity linear.

- **Space Complexity:** O(1)
  - The solution uses a constant amount of extra space for a few variables. This space does not grow with the size of the input array.
