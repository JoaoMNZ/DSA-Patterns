# <a href="https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/" target="_blank">153. Find Minimum in Rotated Sorted Array</a>

### Problem Statement
Suppose an array of length `n` sorted in ascending order is rotated between 1 and `n` times. Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.

You must write an algorithm that runs in `O(log n)` time.

### Approach
Since the array is partially sorted and the required time complexity is `O(log n)`, the solution is a modified **Binary Search**.

The key insight is that in a rotated sorted array, one half of the array (relative to the middle element) will always be normally sorted. We can use this property to determine which half to discard.

We use a `start` and an `end` pointer. In each step of our loop, we find the `mid` element and compare it to the element at the `end` pointer:

1.  **If `numbers[mid] > numbers[end]`:** This tells us that the pivot point (where the smallest numbers begin) must be in the right half of our search space. Therefore, we can safely discard the left half by moving `start` to `mid + 1`.
    
    
2.  **If `numbers[mid] <= numbers[end]`:** This means the section from `mid` to `end` is normally sorted. The minimum element could be `mid` itself or it could be in the left half. So, we discard the right half by moving `end` to `mid`.

This process continues, cutting the search space in half each time, until `start` and `end` meet. At that point, they will both be pointing to the minimum element in the array.

### Implementation
```java
class Solution {
    public int findMin(int[] numbers) {
        int start = 0;
        int end = numbers.length - 1;

        while (start < end) {
            int mid = start + (end - start) / 2;

            if (numbers[mid] > numbers[end]) {
                start = mid + 1;
            } else {
                end = mid;
            }
        }

        return numbers[start];
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(log n)
    -   This is a Binary Search algorithm. In each iteration, we discard half of the remaining search space, which leads to a logarithmic time complexity.

-   **Space Complexity:** O(1)
    -   The solution only uses a few variables for the pointers. The space used is constant and does not grow with the size of the input array.
