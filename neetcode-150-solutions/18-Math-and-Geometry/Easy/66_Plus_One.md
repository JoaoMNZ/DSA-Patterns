# <a href="https://leetcode.com/problems/plus-one/" target="_blank">66. Plus One</a>

### Problem Statement
You are given a large integer represented as an integer array `digits`, where each `digits[i]` is the `i`th digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's.

Increment the large integer by one and return the resulting array of digits.

### Approach
This problem simulates the process of manual addition. We start from the last digit (the least significant) and move backwards.

1.  **Iterate Backwards:** We loop through the array from the last index down to 0.
2.  **Simple Increment:** If the current digit is **less than 9**, we simply add 1 to it and return the array immediately. No carry-over is needed, so the job is done.
3.  **Carry Over:** If the current digit is **9**, adding 1 makes it 10. We set the current digit to `0` and continue the loop to add the "carry" to the next digit on the left.
4.  **Edge Case (All 9s):** If the loop completes without returning, it means every digit was a 9 (e.g., `999` became `000`). We need to handle the overflow by creating a new array of size `n + 1`. We set the first digit to `1` (representing `1000`) and return it.

### Implementation
```java
class Solution {
    public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i--) {
            if (digits[i] < 9) {
                digits[i]++;
                return digits;
            }
            digits[i] = 0;
        }

        int[] newArray = new int[digits.length + 1];
        newArray[0] = 1;
        return newArray;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of digits in the array.

-   **Time Complexity:** O(n)
    -   In the worst case (e.g., `999`), we iterate through all `n` digits.

-   **Space Complexity:** O(1)
    -   The result storage is not counted towards auxiliary space complexity.
