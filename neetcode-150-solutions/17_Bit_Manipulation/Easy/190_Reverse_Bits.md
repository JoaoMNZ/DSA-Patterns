# <a href="https://leetcode.com/problems/reverse-bits/" target="_blank">190. Reverse Bits</a>

### Problem Statement
Reverse bits of a given 32 bits unsigned integer.

### Approach
To reverse the bits, we iterate through all 32 positions of the integer. The goal is to take the bit at the original position `j` and move it to the reversed position `31 - j`.

1.  We loop 32 times, with `i` representing the target position (from 31 down to 0).
2.  In each iteration, we extract the **Least Significant Bit (LSB)** of `n` using `n & 1`.
3.  We shift this extracted bit to the left by `i` positions (`<< i`) to place it in its correct reversed spot.
4.  We add this value to our `result`.
5.  Finally, we right-shift `n` (`n >>= 1`) to discard the processed bit and bring the next bit to the LSB position for the next iteration.

### Implementation
```java
class Solution {
    public int reverseBits(int n) {
        int result = 0;
        for(int i = 31 ; i >= 0 ; i--){
            result += (n & 1) << i;
            n >>= 1;
        }
        return result;
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(1)
    -   The algorithm runs a fixed loop of 32 iterations, regardless of the input value.

-   **Space Complexity:** O(1)
    -   We use a single integer variable `result` to store the answer.
