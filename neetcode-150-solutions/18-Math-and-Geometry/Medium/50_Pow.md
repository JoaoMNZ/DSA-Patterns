# <a href="https://leetcode.com/problems/powx-n/" target="_blank">50. Pow(x, n)</a>

### Problem Statement
Implement `pow(x, n)`, which calculates `x` raised to the power `n` (i.e., `x^n`).

### Approach
The brute force approach of multiplying `x` by itself `n` times would take `O(n)` time, which is too slow for large exponents. We can optimize this using **Exponentiation by Squaring** (Recursion).

The key insight is that any exponentiation can be broken down into smaller subproblems by halving the exponent:
1.  **Even Exponent:** `x^n = (x^(n/2)) * (x^(n/2))`. We can calculate `x^(n/2)` once and square it.
2.  **Odd Exponent:** `x^n = (x^(n/2)) * (x^(n/2)) * x`. We square the result of the half-power and multiply by `x` one more time.

We also need to handle edge cases:
-   **Negative Exponent:** `x^-n = 1 / x^n`. We can calculate the power for the positive `n` and then return the reciprocal.
-   **Base Case:** Anything to the power of 0 is 1.

### Implementation
```java
public class Solution {
    public double myPow(double x, int n) {
        if (x == 0) 
            return 0;
        
        if (n == 0) 
            return 1;
        
        double res = helper(x, Math.abs((long) n));
        
        return (n >= 0) ? res : 1 / res;
    }

    private double helper(double x, long n) {
        if (n == 0) 
            return 1;
        
        double half = helper(x, n / 2);

        return (n % 2 == 0) ? half * half : x * half * half;
    }
}
``` 

### Complexity Analysis
Let `n` be the exponent.

-   **Time Complexity:** O(log n)
    -   Since we divide the exponent by 2 in every recursive call, the number of operations is proportional to the number of bits in `n`, which is logarithmic.

-   **Space Complexity:** O(log n)
    -   The space complexity is determined by the height of the recursion stack, which goes as deep as the number of times we can divide `n` by 2.
