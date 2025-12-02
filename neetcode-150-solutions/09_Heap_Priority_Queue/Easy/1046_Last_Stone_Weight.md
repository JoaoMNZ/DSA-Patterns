# <a href="https://leetcode.com/problems/last-stone-weight/" target="_blank">1046. Last Stone Weight</a>

### Problem Statement
You are given an array of integers `stones` where `stones[i]` is the weight of the `i`th stone.

We are playing a game with the stones. On each turn, we choose the **heaviest two stones** and smash them together. Suppose the heaviest two stones have weights `x` and `y` with `x <= y`. The result of this smash is:
-   If `x == y`, both stones are destroyed.
-   If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.

At the end of the game, there is **at most one stone** left. Return *the weight of the last remaining stone*. If there are no stones left, return `0`.

### Approach
This problem is a simulation that requires repeated access to the **maximum** elements of a collection. A **Max-Heap** is the perfect data structure for this because it allows us to efficiently retrieve and remove the largest element in `O(log n)` time.

The simulation proceeds as follows:
1.  **Build Heap:** We insert all the stones into our Max-Heap.
2.  **Smash Loop:** While the heap has more than one stone:
    -   We extract the heaviest stone (`y`) and the second heaviest stone (`x`).
    -   We compare them. If they are not equal (`y != x`), we put the remainder (`y - x`) back into the heap.
    -   If they are equal, both are destroyed (we do nothing).
3.  **Result:** When the loop finishes, if the heap is empty, return `0`. Otherwise, return the single stone remaining.

In this implementation, the Max-Heap logic (`siftUp`, `siftDown`, `extractMax`) is implemented manually using an `ArrayList`.

### Implementation
```java
import java.util.List;
import java.util.ArrayList;

class Solution {
    class MaxHeap {
        List<Integer> heap;

        public MaxHeap() {
            heap = new ArrayList<>();
        }

        public void add(int val) {
            heap.add(val);
            siftUp(heap.size() - 1);
        }

        public void siftUp(int childIndex) {
            if (childIndex == 0)
                return;

            int parentIndex = (childIndex - 1) / 2;

            if (heap.get(parentIndex) >= heap.get(childIndex))
                return;
            
            swap(childIndex, parentIndex);
            siftUp(parentIndex);
        }

        public void siftDown(int parentIndex) {
            int leftChild = parentIndex * 2 + 1;
            int rightChild = parentIndex * 2 + 2;

            if (leftChild >= heap.size())
                return;
            
            int greatestChild = rightChild < heap.size() && heap.get(rightChild) >= heap.get(leftChild) ? rightChild : leftChild;

            if (heap.get(parentIndex) >= heap.get(greatestChild))
                return;
            
            swap(greatestChild, parentIndex);
            siftDown(greatestChild);
        }

        public void swap(int i, int j) {
            int temp = heap.get(j);
            heap.set(j, heap.get(i));
            heap.set(i, temp);
        }

        public int extractMax() {
            if (heap.size() == 0)
                return -1;

            int max = heap.get(0);
            int last = heap.remove(heap.size() - 1);

            if (heap.size() > 0) {
                heap.set(0, last);
                siftDown(0);
            }

            return max;
        }

        public int size() {
            return heap.size();
        }
    }
    
    public int lastStoneWeight(int[] stones) {
        MaxHeap maxHeap = new MaxHeap();
        for (int stone : stones)
            maxHeap.add(stone);

        while (maxHeap.size() > 1) {
            int y = maxHeap.extractMax();
            int x = maxHeap.extractMax();

            if (y != x) {
                maxHeap.add(y - x);
            }
        }

        return maxHeap.size() == 0 ? 0 : maxHeap.extractMax();
    }
}
``` 

### Complexity Analysis
Let `n` be the number of stones.

-   **Time Complexity:** O(n log n)
    -   Building the initial heap involves adding `n` stones, which takes `O(n log n)`. The main loop runs at most `n` times (each step removes at least one stone). Inside the loop, extracting max and adding takes `O(log n)`. Thus, the total complexity is `O(n log n)`.

-   **Space Complexity:** O(n)
    -   We use an `ArrayList` within our manual `MaxHeap` class to store all `n` stones.
