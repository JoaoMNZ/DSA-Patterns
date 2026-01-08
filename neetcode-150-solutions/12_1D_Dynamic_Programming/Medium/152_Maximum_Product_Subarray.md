# <a href="https://leetcode.com/problems/maximum-product-subarray/" target="_blank">152. Maximum Product Subarray</a>

### Problem Statement
Given an integer array `nums`, find a subarray that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

### Approach
This problem is a variation of **Kadane's Algorithm**. However, unlike the "Maximum Subarray Sum" problem, simply tracking the maximum is not enough.

**The Twist: Negative Numbers.**
* Multiplication by a negative number flips the sign.
* A very small number (large negative value) multiplied by a negative number becomes a very large positive number.
* Therefore, the maximum product at the current position depends on both the maximum *and* the minimum product ending at the previous position.

**Algorithm:**
1.  We maintain two variables `max` and `min` representing the maximum and minimum product of a subarray ending at the current position.
2.  When iterating through `nums`:
    * If the current number is negative, `max` and `min` could swap roles (the max becomes min and vice-versa).
    * To handle this cleanly, we calculate three candidates for the new max/min:
        * The current number itself (starting a new subarray).
        * `current number * previous max` (extending the previous max).
        * `current number * previous min` (extending the previous min - crucial for two negatives making a positive).
3.  We update the global result `res` at every step.

### Implementation
```java
class Solution {
    public int maxProduct(int[] nums) {
        int res = nums[0];

        int min = 1;
        int max = 1;

        for(int num : nums){
            int temp = num * max;
            max = Math.max(Math.max(temp, num * min), num);
            min = Math.min(Math.min(temp, num * min), num);
            res = Math.max(res, max);
        }

        return res;
    }
}
``` 

### Complexity Analysis
Let `N` be the number of elements in the array `nums`.

-   **Time Complexity:** $O(N)$
    -   We iterate through the array exactly once, performing constant-time operations at each step.

-   **Space Complexity:** $O(1)$
    -   We only use a fixed number of variables (`res`, `min`, `max`, `temp`) regardless of the input size.
