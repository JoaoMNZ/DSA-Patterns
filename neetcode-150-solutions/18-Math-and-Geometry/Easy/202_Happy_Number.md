# <a href="https://leetcode.com/problems/happy-number/" target="_blank">202. Happy Number</a>

### Problem Statement
Write an algorithm to determine if a number `n` is happy.

A happy number is a number defined by the following process:
1.  Starting with any positive integer, replace the number by the sum of the squares of its digits.
2.  Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
3.  Those numbers for which this process ends in 1 are happy.

Return `true` if `n` is a happy number, and `false` if not.

### Approach
This problem requires two main logic components: extracting digits to calculate the sum of squares, and detecting an infinite loop.

1.  **Digit Calculation:** To sum the squares of digits, we use arithmetic. We extract the last digit using `n % 10` and then remove that digit from the number using `n / 10`. We repeat this until `n` becomes 0.
2.  **Cycle Detection:** The problem states that the sequence will either reach 1 or loop endlessly. This effectively turns the problem into a cycle detection problem, similar to a Linked List. Instead of using a `HashSet` (which uses extra memory), we can use **Floyd's Cycle-Finding Algorithm** (Slow and Fast Pointers).
    -   The `slow` pointer moves one step (calculates the next sum) at a time.
    -   The `fast` pointer moves two steps at a time.
    -   If there is a cycle, the `fast` and `slow` pointers are guaranteed to meet.
    -   If they meet at the number `1`, the number is happy. If they meet at any other number, it means we are stuck in a loop, so the number is not happy.

### Implementation
```java
class Solution {
    public boolean isHappy(int n) {
        int fast = sumOfSquares(n);
        int slow = n;
        while(fast != slow){
            fast = sumOfSquares(fast);
            fast = sumOfSquares(fast);
            slow = sumOfSquares(slow);
        }
        return fast == 1;
    }

    public int sumOfSquares(int n){
        int sum = 0;
        while(n > 0){
            sum += Math.pow(n % 10, 2);
            n /= 10;
        }
        return sum;
    }
}
``` 

### Complexity Analysis
Let `n` be the input number.

-   **Time Complexity:** O(log n)
    -   Calculating the next number takes time proportional to the number of digits in `n`, which is `log n`.ely quickly, so the total time complexity remains logarithmic.

-   **Space Complexity:** O(1)
    -   We only use two variables (`slow` and `fast`) to detect the cycle, regardless of the input size.
