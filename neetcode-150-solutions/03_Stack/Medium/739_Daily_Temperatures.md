# <a href="https://leetcode.com/problems/daily-temperatures/" target="_blank">739. Daily Temperatures</a>

### Problem Statement
Given an array of integers `temperatures` representing the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `i`th day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

### Approach
We can solve this problem efficiently with a **monotonic stack**. The key idea is to process the temperatures from **right to left**, so that at each step we already know the "future days".

The stack will store **indices of days**, and it is maintained in a way that the temperatures at those indices are in **strictly decreasing order** (from top to bottom).

For each day `i` (looping backwards):
1. While the stack is not empty and the current day's temperature is **greater than or equal to** the temperature at the index on the top of the stack, we `pop()` from the stack. These popped days are useless because day `i` blocks them as a future warmer option.  
2. After cleaning the stack:
   - If the stack is empty, there is no warmer day ahead → `answer[i] = 0`.
   - Otherwise, the index on the top is the **next warmer day**. We compute the distance as `stack.peek() - i`.
3. Finally, we `push(i)` onto the stack, making the current day a candidate for earlier days.


### Implementation
```java
import java.util.Deque;
import java.util.ArrayDeque;

class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int[] daysToWait = new int[temperatures.length];
        Deque<Integer> indicesStack = new ArrayDeque<>();

        for (int day = temperatures.length - 1; day >= 0; day--) {
            while (!indicesStack.isEmpty() && temperatures[day] >= temperatures[indicesStack.peek()]) {
                indicesStack.pop();
            }
            if (!indicesStack.isEmpty()) {
                daysToWait[day] = indicesStack.peek() - day;
            }
            indicesStack.push(day);
        }

        return daysToWait;
    }
}
```

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   Although there is a nested `while` loop, each index is pushed onto the stack and popped from the stack at most once. This means each element is processed a constant number of times on average, leading to a single pass overall.

-   **Space Complexity:** O(n)
    -   In the worst-case scenario (e.g., an array of strictly decreasing temperatures like `[80, 70, 60, 50]`), the stack will grow to hold all `n` indices.
