# <a href="https://leetcode.com/problems/binary-search/description/" target="_blank">704. Binary Search</a>

### Problem Statement
Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

### Approach
Because the array is **sorted**, we can use a very efficient searching method:
1.  We use two pointers, `left` starting at the beginning of the array and `right` at the end.
2.  We find the `mid` index between them.
3.  We check the number at the `mid` index:
    - If `nums[mid]` is our `target`, we found it and can return the index.
    - If `nums[mid]` is **greater** than our `target`, we know the target must be in the left half. So, we can ignore the entire right half by moving our `right` pointer to `mid - 1`.
    - If `nums[mid]` is **less** than our `target`, we know the target must be in the right half. So, we can ignore the entire left half by moving our `left` pointer to `mid + 1`.
4.  We repeat this process, cutting the search area in half each time, until our `left` and `right` pointers cross. If the loop finishes, it means the number isn't in the array, and we return -1.

### Implementation
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while(left <= right){
            int mid = (left + right) / 2;
            if(nums[mid] > target){
                right = mid - 1;
            }else if(nums[mid] < target){
                left = mid + 1;
            }else{
                return mid;
            }
        }
        return -1;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(log n)
  - With each step, we eliminate half of the remaining numbers to search. This "divide and conquer" strategy is what makes the algorithm extremely fast, with a logarithmic time complexity.

- **Space Complexity:** O(1)
  - We only use a few variables to keep track of the pointers (`left`, `right`, `mid`). The amount of memory used does not grow with the size of the input array.
