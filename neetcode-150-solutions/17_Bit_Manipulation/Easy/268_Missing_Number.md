# <a href="https://leetcode.com/problems/missing-number/" target="_blank">268. Missing Number</a>

### Problem Statement
Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the only number in the range that is missing from the array.

### Approach
We can solve this efficiently using the **XOR** bitwise operator (`^`). The solution relies on two key properties:
1.  **Self-Inverse:** A ^ A = 0.
2.  **Identity:** A ^ 0 = A.

We know the full range of numbers should be from `0` to `n`. The input array contains `n` numbers from this range, with one missing.

If we XOR all the numbers in the array, and also XOR all the numbers in the expected range `[0, n]`, every number that is present in the array will appear twice (once in the array, once in the range sequence). These pairs will cancel each other out (becoming 0). The only number left will be the one that appeared only once—the missing number from the range.

### Implementation
```java
class Solution {
    public int missingNumber(int[] nums) {
        int result = nums.length;

        for (int i = 0; i < nums.length; i++) {
            result ^= nums[i] ^ i;
        }

        return result;
    }
}
``` 

### Complexity Analysis
Let `n` be the length of the array.

-   **Time Complexity:** O(n)
    -   We iterate through the array exactly once.

-   **Space Complexity:** O(1)
    -   We use only a single integer variable `result` to store the running XOR sum.
