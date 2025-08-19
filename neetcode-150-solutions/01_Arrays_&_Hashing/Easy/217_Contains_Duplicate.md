# <a href="https://leetcode.com/problems/contains-duplicate/" target="_blank">217. Contains Duplicate</a>

### Problem Statement
Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

### Approach
The core idea is to leverage a data structure that ensures uniqueness. A `HashSet` is perfect for this, as it doesn't allow duplicate elements and provides constant-time average complexity for lookups.

The algorithm is straightforward:
1.  Initialize an empty `HashSet`.
2.  Iterate through each element in the `nums` array.
3.  For each element, attempt to add it to the set. The `.add()` method in Java's `Set` returns `false` if the element already exists.
4.  If `.add()` returns `false`, we've found a duplicate and can immediately return `true`.
5.  If the loop completes, it means no duplicates were found, and we can return `false`.

### Implementation
```java
import java.util.Set;
import java.util.HashSet;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<>();

        for(int i = 0; i < nums.length; i++){
            if(!set.add(nums[i])){
                return true;
            }
        }
        return false;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n)
  - In the worst-case scenario, we must iterate through all `n` elements of the array. The `add` operation for a `HashSet` is O(1) on average.

- **Space Complexity:** O(n)
  - In the worst-case scenario (an array with all unique elements), we will store all `n` elements in the `HashSet`.
