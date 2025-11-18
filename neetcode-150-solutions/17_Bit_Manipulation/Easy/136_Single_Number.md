# <a href="https://leetcode.com/problems/single-number/" target="_blank">136. Single Number</a>

### Problem Statement
Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

### Approach
The most efficient solution leverages the **Bitwise XOR** (`^`) operator. To understand why this works, we look at three key properties of XOR:
1.  **Identity:** A ^ 0 = A
2.  **Self-Inverse:** A ^ A = 0
3.  **Commutativity:** The order of operations does not matter: A ^ (B ^ C) = (A ^ B) ^ C.

Since every number appears exactly twice except for one, if we XOR all the numbers in the array together, the duplicate pairs will cancel each other out (becoming 0) and the final result will inevitably be the single unique number.

### Implementation
```java
class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;
        for (int num : nums) {
            result ^= num;
        }
        return result;
    }
}
``` 

### Complexity Analysis
Let `n` be the length of the `nums` array.

-   **Time Complexity:** O(n)
    -   We iterate through the array exactly once, performing a constant-time XOR operation for each element.

-   **Space Complexity:** O(1)
    -   We use only a single variable `result` to store the running XOR sum.
