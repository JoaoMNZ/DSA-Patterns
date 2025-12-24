# <a href="https://leetcode.com/problems/sliding-window-maximum/" target="_blank">239. Sliding Window Maximum</a>

### Problem Statement
You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

### Approach
We need to access the maximum element in the current window efficiently as it moves. A standard heap would take $O(\log k)$ per move, resulting in $O(N \log k)$ total. We can optimize this to $O(N)$ using a **Monotonic Decreasing Queue** (Deque).

1.  **The Deque Structure:** We use a `Deque` to store **indices** of elements.
    -   **Invariant:** The values corresponding to the indices in the deque are strictly in **decreasing order**.
    -   This means the **front** of the deque (`peekFirst`) always holds the index of the **largest element** in the current window.

2.  **Step-by-Step Logic:**
    -   **Pop Smaller Elements (Maintain Monotonicity):** Before adding the current element `nums[right]`, we look at the back of the deque. If the last element is smaller than or equal to `nums[right]`, it can never be the maximum again (because `nums[right]` is larger and will stay in the window longer). So, we remove it. We repeat this until the deque is monotonic.
    -   **Add Current:** We add the current index `right` to the back.
    -   **Remove Outdated Maximums:** We check the front of the deque. If the index stored there is `< left` (i.e., it is outside the current window range), we remove it.
    -   **Record Result:** Once the window reaches size `k` (i.e., `right - left + 1 >= k`), the front of the deque is our maximum for this window.



### Implementation
```java
import java.util.Deque;
import java.util.ArrayDeque;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        Deque<Integer> deque = new ArrayDeque<>();
        int[] max = new int[n - k + 1];

        int left = 0, right = 0;
        while(right < n){
            while(!deque.isEmpty() && nums[right] >= nums[deque.peekLast()]){
                deque.removeLast();
            }
            deque.addLast(right);

            if(deque.peekFirst() < left){
                deque.removeFirst();
            }

            if((right - left + 1) >= k){
                max[left] = nums[deque.peekFirst()];
                left++;
            }
            
            right++;
        }
            
        return max;
    }
}
``` 

### Complexity Analysis
Let `N` be the number of elements in the array `nums`.

-   **Time Complexity:** $O(N)$
    -   Each element is added to the deque exactly once.
    -   Each element is removed from the deque at most once.
    -   Therefore, the operations are linear relative to the array size.

-   **Space Complexity:** $O(k)$
    -   In the worst case (e.g., a sorted decreasing array), the deque might store up to `k` indices (all elements in the current window).
