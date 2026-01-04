# <a href="https://leetcode.com/problems/find-median-from-data-stream/" target="_blank">295. Find Median from Data Stream</a>

### Problem Statement
The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.

For example, for `arr = [2,3,4]`, the median is `3`.
For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the `MedianFinder` class:
* `MedianFinder()` initializes the `MedianFinder` object.
* `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
* `double findMedian()` returns the median of all elements so far. Answers within `10^-5` of the actual answer will be accepted.

### Approach
We need to find the middle value of a data stream efficiently. Sorting the array every time would take $O(N \log N)$, which is too slow.

The optimal approach uses **Two Heaps** to divide the data stream into two halves:
1.  **Max-Heap (`maxHeap`):** Stores the **smaller half** of the numbers. We want easy access to the largest number in this half (the one closest to the median).
2.  **Min-Heap (`minHeap`):** Stores the **larger half** of the numbers. We want easy access to the smallest number in this half.



**Balancing Strategy:**
* We maintain the invariant that `maxHeap` either has the same number of elements as `minHeap` or exactly one more element.
* **Adding a Number:**
    * If the total size is **even**: We intend to add to the `maxHeap`. However, if the new number belongs to the upper half (it's larger than `minHeap.peek()`), we swap: move the smallest element from `minHeap` to `maxHeap` and put the new number in `minHeap`.
    * If the total size is **odd**: We intend to add to the `minHeap`. However, if the new number belongs to the lower half (it's smaller than `maxHeap.peek()`), we swap: move the largest element from `maxHeap` to `minHeap` and put the new number in `maxHeap`.
* **Finding Median:**
    * If total size is **even**: Median is the average of the two roots (`maxHeap.peek() + minHeap.peek() / 2.0`).
    * If total size is **odd**: Median is simply the root of the `maxHeap`.

### Implementation
```java
import java.util.Queue;
import java.util.PriorityQueue;
import java.util.Collections;

class MedianFinder {
    Queue<Integer> maxHeap;
    Queue<Integer> minHeap;

    public MedianFinder() {
        maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        minHeap = new PriorityQueue<>();
    }
    
    public void addNum(int num) {
        if(this.size() % 2 == 0){
            if(!minHeap.isEmpty() && num > minHeap.peek()){
                maxHeap.offer(minHeap.poll());
                minHeap.offer(num);
            }else{
                maxHeap.offer(num);
            }
        }else{
            if(!maxHeap.isEmpty() && num < maxHeap.peek()){
                minHeap.offer(maxHeap.poll());
                maxHeap.offer(num);
            }else{
                minHeap.offer(num);
            }
        }
    }
    
    public double findMedian() {
        if(this.size() % 2 == 0){
            return (maxHeap.peek() + minHeap.peek()) / 2.0;
        }
        return maxHeap.peek();
    }

    public int size(){
        return maxHeap.size() + minHeap.size();
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

### Complexity Analysis
Let `N` be the number of elements added to the data structure.

-   **Time Complexity:**
    -   `addNum`: $O(\log N)$. Insertion and deletion in a heap takes logarithmic time relative to the number of elements.
    -   `findMedian`: $O(1)$. Accessing the root (`peek`) of a heap is a constant time operation.

-   **Space Complexity:** $O(N)$
    -   We store every number from the stream exactly once across the two heaps.
