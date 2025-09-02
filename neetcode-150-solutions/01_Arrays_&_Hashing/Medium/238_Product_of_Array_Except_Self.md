# <a href="https://leetcode.com/problems/product-of-array-except-self/" target="_blank">238. Product of Array Except Self</a>

### Problem Statement
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

### Approach
The problem asks for the product of everything to the left of an element multiplied by the product of everything to its right. A clever way to achieve this in O(1) space (excluding the output array) is with a two-pass approach.

1.  **First Pass (Prefix Products):** We create our `result` array. We iterate from left to right, and for each index `i`, we calculate the product of all numbers *before* it. We store this prefix product in `result[i]`. After this pass, `result[i]` holds the product of all elements to its left.

2.  **Second Pass (Suffix Products):** Now, we need to factor in the products of the elements to the right. We can do this by iterating from right to left. We use a single variable, `suffixProduct`, to keep track of the cumulative product of elements to the right. In each step, we first multiply the current prefix product in `result[i]` by the `suffixProduct` to get the final answer. Then, we update the `suffixProduct` by multiplying it with the current number `nums[i]` for the next iteration.

This way, we build the final result in place without needing a separate suffix array.

### Implementation
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];

        result[0] = 1;
        for (int i = 1; i < n; i++) {
            result[i] = nums[i - 1] * result[i - 1];
        }

        int suffixProduct = 1;
        for (int i = n - 1; i >= 0; i--) {
            result[i] = result[i] * suffixProduct;
            suffixProduct *= nums[i];
        }

        return result;
    }
}
```

### Complexity Analysis
- **Time Complexity:** O(n)
  - The algorithm consists of two separate loops that each iterate through the array once. This results in a time complexity of O(n) + O(n), which simplifies to O(n).

- **Space Complexity:** O(1)
  - According to the problem description, the output array does not count towards extra space complexity. Besides the `result` array, the solution uses only a few variables, resulting in O(1) auxiliary space.
