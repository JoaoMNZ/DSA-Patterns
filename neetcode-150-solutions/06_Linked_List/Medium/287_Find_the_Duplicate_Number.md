# <a href="https://leetcode.com/problems/find-the-duplicate-number/" target="_blank">287. Find the Duplicate Number</a>

### Problem Statement
Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive. There is only one repeated number in `nums`. Return this repeated number.

*(Note: The following solution modifies the input array to achieve an O(1) space solution, ignoring one of the problem's optional constraints in favor of a creative in-place approach.)*

### Approach
This approach creatively uses the input array itself as a `HashMap` to find the duplicate without using extra space.

The main idea is to "mark" an index when we see the number corresponding to it.
1.  **Mapping:** We can map each number `num` to an `index` in the array. This implementation uses the modulo operator for this: `index = num % size`. The properties of the modulo operator ensure we can still find the correct base index even if the value has been modified.
2.  **Marking:** To mark an index as "visited," we modify the value at that index by adding the array's `size` to it.
3.  **Detecting:** We iterate through the array. For each `num`, we calculate its corresponding `index`. We then check the value at `nums[index]`. If this value is already greater than the array's `size`, it means we have already added `size` to it before. This tells us we've seen this index previously, and therefore the number that maps to this index is the duplicate.

### Implementation
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int size = nums.length;
        for (int num : nums) {
            int index = num % size;
            if (nums[index] > size) {
                return index;
            }
            nums[index] += size;
        }
        return -1;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of elements in the array.

-   **Time Complexity:** O(n)
    -   We iterate through the array once.

-   **Space Complexity:** O(1)
    -   The algorithm modifies the array in-place and uses a constant amount of extra space.
