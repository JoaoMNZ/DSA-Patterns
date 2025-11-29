# <a href="https://leetcode.com/problems/multiply-strings/" target="_blank">43. Multiply Strings</a>

### Problem Statement
Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Note:** You must not use any built-in BigInteger library or convert the inputs to integers directly.

### Approach
This problem can be solved by simulating the standard vertical multiplication algorithm we do on paper.

1.  **Result Array:** The product of two numbers with lengths `m` and `n` can have at most `m + n` digits. We create an integer array `digits` of this size to store the intermediate results.
2.  **Digit-by-Digit Multiplication:** We iterate through both strings from right to left (indices `i` and `j`).
    -   We calculate the product of the two current digits: `mul = (num1[i] - '0') * (num2[j] - '0')`.
    -   **Positioning:** The result of multiplying the digit at index `i` with the digit at index `j` will contribute to indices `i + j` (high digit) and `i + j + 1` (low digit) in our `digits` array.
    -   We add the `mul` result to the value already at `digits[i + j + 1]`.
3.  **Carry Handling:** After adding, we process the carry immediately:
    -   The new carry to be added to the left position (`i + j`) is `digits[i + j + 1] / 10`.
    -   The digit that remains at the current position is `digits[i + j + 1] % 10`.
4.  **Formatting:** Finally, we convert the `digits` array into a string, making sure to skip any leading zeros.

### Implementation
```java
class Solution {
    public String multiply(String num1, String num2) {
        if (num1.equals("0") || num2.equals("0")) {
            return "0";
        }

        int[] digits = new int[num1.length() + num2.length()];
        
        for (int i = num1.length() - 1; i >= 0; i--) {
            for (int j = num2.length() - 1; j >= 0; j--) {
                int pos = i + j + 1;
                int multiplication = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
                
                digits[pos] += multiplication;
                digits[pos - 1] += digits[pos] / 10;
                digits[pos] %= 10;
            }
        }
        
        int i = 0;
        while (i < digits.length && digits[i] == 0) {
            i++;
        }
        
        StringBuilder result = new StringBuilder();
        while (i < digits.length) {
            result.append(digits[i]);
            i++;
        }
        return result.toString();
    }
}
``` 

### Complexity Analysis
Let `m` be the length of `num1` and `n` be the length of `num2`.

-   **Time Complexity:** O(m * n)
    -   We use a nested loop where each digit of `num1` is multiplied by each digit of `num2`, resulting in `m * n` operations.

-   **Space Complexity:** O(m + n)
    -   We use an integer array of size `m + n` to store the intermediate digits of the product.
