# <a href="https://leetcode.com/problems/kth-largest-element-in-an-array/" target="_blank">215. Kth Largest Element in an Array</a>

### Problem Statement
Given an integer array `nums` and an integer `k`, return the `k`th largest element in the array.

Note that it is the `k`th largest element in the sorted order, not the `k`th distinct element. Can you solve it without sorting?

### Approach
The logic works by maintaining a ranking of the top `k` largest elements seen so far.
1.  **Min-Heap Structure:** We use a Min-Heap to store elements. The root of a Min-Heap always holds the smallest element currently in the heap.
2.  **Maintaining Top K:** We iterate through the array adding numbers to the heap.
    -   If the heap size exceeds `k` (i.e., we have `k + 1` elements), we remove (`poll`) the smallest element.
    -   By constantly removing the smallest element whenever we go over the limit, we ensure that the heap only retains the "largest" numbers encountered so far.
3.  **The Result:** After processing the entire array, the heap will contain exactly the `k` largest elements. The smallest of these top `k` elements (the root of the heap) is, by definition, the `k`th largest element of the entire array.

### Implementation
```java
import java.util.Queue;
import java.util.PriorityQueue;

class Solution {
    public int findKthLargest(int[] nums, int k) {
        Queue<Integer> minHeap = new PriorityQueue<>();

        for(int num : nums){
            minHeap.offer(num);
            if(minHeap.size() > k){
                minHeap.poll();
            }
        }

        return minHeap.poll();
    }
}
``` 

### Complexity Analysis
Let `n` be the number of elements in the array `nums` and `k` be the target rank.

-   **Time Complexity:** O(n log k)
    -   We iterate through `n` elements. For each element, we perform heap operations (`offer` and potentially `poll`). Since the heap size is capped at `k`, these operations take `O(log k)` time.

-   **Space Complexity:** O(k)
    -   The heap stores at most `k + 1` elements at any point.
