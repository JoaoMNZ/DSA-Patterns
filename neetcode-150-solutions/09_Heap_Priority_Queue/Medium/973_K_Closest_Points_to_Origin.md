# <a href="https://leetcode.com/problems/k-closest-points-to-origin/" target="_blank">973. K Closest Points to Origin</a>

### Problem Statement
Given an array of points where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., $\sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2}$).

You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

### Approach
To find the `k` closest points efficiently without sorting the entire array, we can maintain a **Max-Heap** of fixed size `k`.

1. **Distance Calculation:** We calculate the squared Euclidean distance x^2 + y^2 for comparison, avoiding the square root operation (it doesn't change the result).
2. **Heap Logic (Max-Heap of Size K):**
   -  We iterate through each point.
   -  We add the point to the heap. To effectively create a "Max-Heap" behavior using Java's default `PriorityQueue` (which is a Min-Heap), we store the **negative** of the distance. This puts the element with the largest actual distance at the top (smallest negative number).
   -  **Maintain Size K:** Immediately after adding a point, if the heap size exceeds `k`, we remove (`poll`) the top element. Since this is a Max-Heap logic, we are removing the point with the *largest* distance currently in our collection.
3. **Result:** After checking all points, the heap will contain exactly the `k` points with the smallest distances. We extract them to return the result.

### Implementation
```java
import java.util.Queue;
import java.util.PriorityQueue;
import java.util.Comparator;

class Solution {
   public int[][] kClosest(int[][] points, int k) {
       Queue<int[]> maxHeap = new PriorityQueue<>(Comparator.comparing(a -> a[0]));

       for(int[] point : points){
           int distance = point[0] * point[0] + point[1] * point[1];
           maxHeap.offer(new int[]{-distance, point[0], point[1]});
           if(maxHeap.size() > k){
               maxHeap.poll();
           }
       }

       int[][] result = new int[k][2];
       for(int i = 0 ; i < k ; i++){
           int[] point = maxHeap.poll();
           result[i] = new int[]{point[1], point[2]};
       }
       return result;
   }
}
```

### Complexity Analysis
Let `n` be the number of points in the array.

-  **Time Complexity:** O(n log k)
   -  We iterate through all `N` points. The heap operations (`offer` and `poll`) occur on a heap of size `k`, taking `O(log k)` time.

-  **Space Complexity:** O(k)
   -  The heap stores at most `k + 1` elements at any given time.
