# <a href="https://leetcode.com/problems/container-with-most-water/description/" target="_blank">11. Container With Most Water</a>

### Problem Statement
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i`th line are `(i, 0)` and `(i, height[i])`. Find two lines that together with the x-axis form a container, such that the container contains the most water. Return the maximum amount of water a container can store. Notice that you may not slant the container.

### Approach
The area of the container is determined by `width * height`. To maximize this area, we should aim for the largest possible width and height.

A **Two Pointers** approach is ideal for this. We start with one pointer, `left`, at the beginning of the array and another, `right`, at the end. This gives us the maximum possible width.

In each iteration, we calculate the area:
- The `width` is the distance between the pointers (`right - left`).
- The `height` is limited by the shorter of the two lines, so we use `Math.min(height[left], height[right])`.

We keep track of the maximum area found so far.

To potentially find a larger area, we must move one of the pointers. If we move the pointer of the taller line, the width will decrease, and the height is guaranteed to either decrease or stay the same. This can't lead to a better solution. Therefore, the only logical move is to advance the pointer of the **shorter line**, in hopes of finding a taller line that might compensate for the reduced width. This process continues until the pointers meet.

### Implementation
```java
class Solution {
    public int maxArea(int[] height) {
        int result = 0;
        int left = 0;
        int right = height.length - 1;
        
        while (left < right) {
            int area = Math.min(height[left], height[right]) * (right - left);
            result = Math.max(area, result);
            
            if (height[left] > height[right]) {
                right--;
            } else {
                left++;
            }
        }
        return result;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n)
  - The `left` and `right` pointers traverse the array at most once. In each step of the loop, one of the pointers moves inward, ensuring a single pass through the array.

- **Space Complexity:** O(1)
  - The solution uses a fixed number of variables (`result`, `left`, `right`, `area`). The space required does not scale with the size of the input array.
