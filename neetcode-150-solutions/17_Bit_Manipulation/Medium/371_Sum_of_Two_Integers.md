# <a href="https://leetcode.com/problems/sum-of-two-integers/" target="_blank">371. Sum of Two Integers</a>

### Problem Statement
Given two integers `a` and `b`, return the sum of the two integers without using the operators `+` and `-`.

### Approach
This problem requires us to simulate digital logic addition (how a CPU adds numbers at the bit level). We can break addition down into two parts: the "sum without carry" and the "carry" itself.

1.  **Sum without carry (XOR `^`):**
    -   `0 + 0 = 0`
    -   `0 + 1 = 1`
    -   `1 + 0 = 1`
    -   `1 + 1 = 0`
    
This behavior is exactly what the **XOR** operator does. It adds bits but ignores the carry.

2.  **Carry (AND `&` + Shift `<<`):**
    -   A carry is generated only when we add `1 + 1`. This is exactly what the **AND** operator identifies.
    -   A carry always affects the *next* significant bit position. Therefore, we must shift the result of the AND operation one step to the left (`<< 1`).

**The Algorithm:**
We effectively treat `a` as our running total and `b` as our carry.
1.  Calculate the `carry` using `(a & b) << 1`.
2.  Update `a` to contain the partial sum using `a ^ b`.
3.  Update `b` to be the `carry`.
4.  Repeat this process until `b` becomes `0` (meaning there are no more carries to add).

### Implementation
```java
public class Solution {
    public int getSum(int a, int b) {
        while (b != 0) {
            int carry = (a & b) << 1;
            a ^= b;
            b = carry;
        }
        return a;
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(1)
    -   The loop runs until the carry is resolved. In a fixed-size integer system, the carry can propagate at most 32 times.

-   **Space Complexity:** O(1)
    -   We use only two variables, requiring constant extra space.
