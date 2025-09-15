# <a href="https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/" target="_blank">167. Two Sum II - Input Array Is Sorted</a>

### Problem Statement
Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]`.

Return the indices of the two numbers, `index1` and `index2`, **added by one** as an integer array `[index1, index2]`. You may not use the same element twice and your solution must use only constant extra space.

### Approach
Since the array is **sorted** and we need an `O(1)` space solution, the **Two Pointers** technique is the ideal approach.

We start with one pointer, `left`, at the beginning of the array and another pointer, `right`, at the end. We then calculate the sum of the values at these two pointers and compare it to the `target`.

1.  If the `sum` is **greater than** the `target`, our sum is too large. To decrease it, we must move the `right` pointer one step to the left, selecting a smaller number.
2.  If the `sum` is **less than** the `target`, our sum is too small. To increase it, we must move the `left` pointer one step to the right, selecting a larger number.
3.  If the `sum` is **equal to** the `target`, we have found our pair and can return their indices (remembering to add 1 to each as requested by the problem).

We repeat this process, narrowing our search window, until we find the solution.

### Implementation
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;

        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum > target) {
                right--;
            } else if (sum < target) {
                left++;
            } else {
                return new int[] {left + 1, right + 1};
            }
        }
        return new int[0];
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   In the worst case, the `left` and `right` pointers will traverse the entire array once. Each iteration brings the pointers closer together, resulting in a single pass and a linear time complexity.

-   **Space Complexity:** O(1)
    -   The solution uses a constant amount of extra space for the two pointers. The space required does not grow with the size of the input array.
