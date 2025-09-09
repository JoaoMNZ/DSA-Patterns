# <a href="https://leetcode.com/problems/koko-eating-bananas/" target="_blank">875. Koko Eating Bananas</a>

### Problem Statement
There are `n` piles of bananas, the `i`th pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours. Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile and eats `k` bananas. If the pile has less than `k` bananas, she eats all of them and will not eat any more bananas during this hour.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

### Approach
The problem asks for the *minimum* speed `k` within a range of possibilities. This structure is a perfect fit for a **Binary Search** on the answer.

We need to define the search space for the speed `k`:
-   The slowest possible speed is `k=1`.
-   The fastest necessary speed is equal to the largest pile, as any speed higher than that provides no benefit.

Our Binary Search will test different speeds and check if they are valid:
1.  We pick a test speed, `mid`, from our current search range.
2.  We use a helper function, `canFinish`, to calculate the total hours required to eat all bananas at this `mid` speed.
3.  If Koko **can finish** in time (`<= h` hours): This `mid` speed is a possible answer. However, we want the *minimum* possible speed, so we try to find a better (slower) one by searching in the left half of our range (`right = mid - 1`).
4.  If Koko **cannot finish** in time: The `mid` speed is too slow. We must search for a faster speed in the right half of our range (`left = mid + 1`).

This process continues until the search space is exhausted, and our pointer will be at the minimum valid speed.

### Implementation
```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int maxPile = 0;
        for (int pile : piles) {
            maxPile = Math.max(maxPile, pile);
        }
        
        int right = maxPile;
        int left = 1;
        while (left <= right) {
            int speed = left + (right - left) / 2;
            if (canFinish(piles, h, speed)) {
                right = speed - 1;
            } else {
                left = speed + 1;
            }
        }
        return left;
    }

    private boolean canFinish(int[] piles, int h, int speed) {
        long hours = 0;
        for (int pile : piles) {
            // This is a math trick for ceiling division: (a + b - 1) / b
            hours += (pile + speed - 1) / speed;
        }
        return hours <= h;
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(n * log m)
    -   Let `n` be the number of piles and `m` be the maximum number of bananas in a single pile. The Binary Search runs on a range of speeds up to `m`, which takes `O(log m)` iterations. Inside each iteration, the `canFinish` function iterates through all `n` piles. This results in a total time complexity of `O(n * log m)`.

-   **Space Complexity:** O(1)
    -   The algorithm uses a constant amount of extra space for a few variables. The space required does not grow with the size of the input.
