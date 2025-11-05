# <a href="https://leetcode.com/problems/maximum-subarray/" target="_blank">53. Maximum Subarray</a>

### Problem Statement
Given an integer array `nums`, find the `subarray` with the largest sum, and return *its sum*.

### Approach
This problem can be solved efficiently in a single pass using **Kadane's Algorithm**.

The idea is to iterate through the array, maintaining two variables: `maximum` (the largest sum found so far) and `currentSum` (the sum of the subarray ending at the current position).

1.  We initialize `currentSum` to 0 and `maximum` to the smallest possible integer value.
2.  For each number in the array, we add it to `currentSum`.
3.  We check if this `currentSum` is now greater than our overall `maximum`. If it is, we update `maximum`.
4.  **The Key Insight:** If at any point the `currentSum` becomes negative, we reset it to `0`. A subarray with a negative sum will only decrease the total of any future subarray that builds upon it. It is always better to discard that negative portion and start a new subarray from the next element.

### Implementation
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maximum = Integer.MIN_VALUE;
        int currentSum = 0;

        for(int num : nums){
            currentSum += num;
            maximum = Math.max(maximum, currentSum);
            if(currentSum < 0)
                currentSum = 0;
        }

        return maximum;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of elements in the array `nums`.

-   **Time Complexity:** O(n)
    -   We iterate through the array of `n` elements exactly once.

-   **Space Complexity:** O(1)
    -   We only use a few variables to track the sums.
