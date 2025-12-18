# <a href="https://leetcode.com/problems/largest-rectangle-in-histogram/" target="_blank">84. Largest Rectangle in Histogram</a>

### Problem Statement
Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return the area of the largest rectangle in the histogram.

### Approach
To find the largest rectangle efficiently, we need to solve a sub-problem for every bar: **"If I use this bar as the full height of the rectangle, how far can I extend to the left and to the right?"**

We need to find two boundaries for the current bar `h`:
1.  **Left Boundary:** The index of the first bar to the left that is *shorter* than `h`.
2.  **Right Boundary:** The index of the first bar to the right that is *shorter* than `h`.

We can solve this in linear time using a **Monotonic Increasing Stack**.
-   We iterate through the bars and store their **indices** in a stack.
-   **Invariant:** The stack always keeps indices of bars with increasing heights.
-   **Trigger:** When we encounter a current bar (`heights[i]`) that is **smaller** than the bar at the top of the stack (`heights[stack.peek()]`), we know we have found the **Right Boundary** for the stack's top.
-   **Calculation:**
    -   We pop the top index. This is the **Height** of the rectangle.
    -   The current index `i` is the Right Boundary.
    -   The new top of the stack (after popping) is the **Left Boundary**.
    -   `Width = Right Boundary - Left Boundary - 1` (or simply `i` if the stack is empty).
    -   `Area = Height * Width`.

### Implementation
```java
import java.util.Deque;
import java.util.ArrayDeque;

class Solution {
    public int largestRectangleArea(int[] heights) {
        Deque<Integer> stack = new ArrayDeque<>();
        int n = heights.length;
        int maxArea = 0;

        for(int i = 0 ; i <= n ; i++){
            while(!stack.isEmpty() && (i == n || heights[i] < heights[stack.peek()])){
                int height = heights[stack.pop()];
                int width = stack.isEmpty() ? i : i - stack.peek() - 1;
                maxArea = Math.max(maxArea, height * width);
            }
            stack.push(i);
        }
        
        return maxArea;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of bars in the histogram.

-   **Time Complexity:** $O(n)$
    -   Each index is pushed onto the stack exactly once and popped from the stack exactly once. Even though there is a `while` loop inside the `for` loop, the amortized cost is linear.

-   **Space Complexity:** $O(n)$
    -   In the worst case (e.g., a strictly increasing array), the stack will store all `n` indices.
