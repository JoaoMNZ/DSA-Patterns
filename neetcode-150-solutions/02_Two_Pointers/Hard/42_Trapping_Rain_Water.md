# <a href="https://leetcode.com/problems/trapping-rain-water/" target="_blank">42. Trapping Rain Water</a>

### Problem Statement
Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

### Approach
For any specific position `i`, the amount of water it can hold is determined by the shortest of the two tallest walls surrounding it: `min(maxLeft, maxRight) - height[i]`.

We can calculate this efficiently using the **Two Pointers** technique, which reduces space complexity to $O(1)$.

1.  **Pointers:** Initialize `left = 0` and `right = n - 1`.
2.  **Max Height Tracking:** Maintain `leftMax` and `rightMax` to track the highest bars seen so far from the left and right sides, respectively.
3.  **The "Bottleneck" Logic:**
    -   At any step, we compare `leftMax` and `rightMax`.
    -   If `leftMax < rightMax`: We know that for the current `left` position, the **limiting factor** is definitely `leftMax`. Even if there is a much taller wall somewhere far to the right, water height is capped by `leftMax`.
        -   If `height[left] < leftMax`, we trap water: `result += leftMax - height[left]`.
        -   Update `leftMax` and move `left` forward.
    -   If `leftMax >= rightMax`: The limiting factor for the `right` position is `rightMax`.
        -   If `height[right] < rightMax`, we trap water: `result += rightMax - height[right]`.
        -   Update `rightMax` and move `right` backward.
    
This ensures we always calculate water levels based on the guaranteed lower boundary (the bottleneck).

### Implementation
```java
class Solution {
    public int trap(int[] height) {
        int left = 0, right = height.length - 1;
        int leftMax = height[left], rightMax = height[right];
        int result = 0;

        while(left < right){
            if(leftMax < rightMax){
                left++;
                leftMax = Math.max(leftMax, height[left]);
                result += leftMax - height[left];
            }else{
                right--;
                rightMax = Math.max(rightMax, height[right]);
                result += rightMax - height[right];
            }
        }

        return result;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of bars in the elevation map.

-   **Time Complexity:** $O(n)$
    -   We iterate through the array using two pointers, processing each index exactly once.

-   **Space Complexity:** $O(1)$
    -   This approach only uses a few variables (`left`, `right`, `leftMax`, `rightMax`) for storage.
