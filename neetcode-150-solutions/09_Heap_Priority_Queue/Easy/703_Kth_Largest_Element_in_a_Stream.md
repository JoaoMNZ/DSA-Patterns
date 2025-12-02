# <a href="https://leetcode.com/problems/kth-largest-element-in-a-stream/" target="_blank">703. Kth Largest Element in a Stream</a>

### Problem Statement
Design a class to find the `k`th largest element in a stream. Note that it is the `k`th largest element in the sorted order, not the `k`th distinct element.

Implement `KthLargest` class:
-   `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of integers `nums`.
-   `int add(int val)` Appends the integer `val` to the stream and returns the element representing the `k`th largest element in the stream.

### Approach
To efficiently find the `k`th largest element in a dynamic stream, we can use a **Min-Heap** with a fixed size of `k`.

1.  **Why a Min-Heap of size `k`?**
    -   If we keep the `k` largest numbers seen so far in a heap, the *smallest* number among them (the root of the Min-Heap) is exactly the `k`th largest number overall.
    -   Any number smaller than this root doesn't belong in the "top `k`" list, so we can ignore it.

2.  **Manual Heap Implementation:**
    -   Instead of using Java's built-in `PriorityQueue`, this solution implements the heap logic manually using an `ArrayList`.
    -   **`add` logic:**
        -   If the heap isn't full yet (`size < k`), we append the new value and perform a **`siftUp`** to restore the heap property (swapping with parents until it's in the right spot).
        -   If the heap is full (`size == k`), we compare the new `val` with the root (minimum). If `val` is larger, it belongs in the top `k`. We replace the root with `val` and perform a **`siftDown`** to sink the new root to its correct position.
    -   **`siftUp` and `siftDown`:** These are the core helper functions that ensure the smallest element always bubbles up to index 0.

### Implementation
```java
import java.util.Array;
import java.util.ArrayList;

class KthLargest {
    List<Integer> heap;
    int maxSize;

    public KthLargest(int k, int[] nums) {
        heap = new ArrayList<>();
        maxSize = k;
        for(int num : nums){
            add(num);
        }
    }
    
    public int add(int val) {
        if(heap.size() < maxSize){
            heap.add(val);
            siftUp(heap.size() - 1);
        } else {
            if(val > heap.get(0)){
                heap.set(0, val);
                siftDown(0);
            }
        }
        
        return heap.get(0);
    }

    public void siftUp(int childIndex){
        if(childIndex == 0)
            return;
        
        int parentIndex = (childIndex - 1) / 2;

        if(heap.get(childIndex) < heap.get(parentIndex)){
            swap(childIndex, parentIndex);
            siftUp(parentIndex);
        }
    }

    public void siftDown(int parentIndex){
        int leftChild = parentIndex * 2 + 1;
        int rightChild = parentIndex * 2 + 2;

        if(leftChild >= heap.size())
            return;

        int lowestChild = rightChild < heap.size() && heap.get(rightChild) <= heap.get(leftChild) ? rightChild : leftChild;

        if(heap.get(lowestChild) >= heap.get(parentIndex))
            return;

        swap(lowestChild, parentIndex);
        siftDown(lowestChild);
    }

    public void swap(int i, int j){
        int temp = heap.get(j);
        heap.set(j, heap.get(i));
        heap.set(i, temp);
    }
}
``` 

### Complexity Analysis
let `m` be the number of calls to `add`, and `k` be the target rank.

-   **Time Complexity: O(m * log k)**
    -  `siftUp` and `siftDown` operations traverse the height of the heap, which is logarithmic relative to the heap size (`k`) and we call these functions m times.

-   **Space Complexity:** O(k)
    -   We use an `ArrayList` to store the heap elements. Since we limit the size to `maxSize`, the space required is `O(k)`.
