# <a href="https://leetcode.com/problems/number-of-1-bits/" target="_blank">191. Number of 1 Bits</a>

### Problem Statement
Write a function that takes the binary representation of a positive integer and returns the number of **set bits** it has (also known as the **Hamming weight**).

### Approach
We need to count how many times `1` appears in the binary representation of `n`. We can check the bits one by one, starting from the the rightmost bit.

1.  **Check:** We check if the last bit is `1` by performing a bitwise AND operation with 1 (`n & 1`). If the last bit is `1`, the result is `1`; otherwise, it's `0`. We add this result to our `hamming` count.
2.  **Shift:** To check the next bit, we right-shift the number by one position (`n >> 1`). This discards the bit we just checked and moves the next bit into the last position. 
3.  **Repeat:** We repeat this process until `n` becomes `0`, meaning there are no more set bits to count.

### Implementation
```java
class Solution {
    public int hammingWeight(int n) {
        int hamming = 0;
        while(n != 0){
            hamming += n & 1;
            n = n >> 1;
        }
        return hamming;
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(1)
    -   The input integer is of a fixed size (32 bits). The loop runs at most 32 times.
-   **Space Complexity:** O(1)
    -   We use only a single variable `hamming` to store the count.
