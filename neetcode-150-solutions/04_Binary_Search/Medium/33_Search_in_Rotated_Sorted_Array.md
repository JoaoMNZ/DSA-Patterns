# <a href="https://leetcode.com/problems/search-in-rotated-sorted-array/" target="_blank">33. Search in Rotated Sorted Array</a>

### Problem Statement
There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly **left rotated** at an unknown index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed).
For example, `[0,1,2,4,5,6,7]` might be rotated at index 3 and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

### Approach
Even though the array is rotated, it still preserves a crucial property: **at any pivot point (middle), at least one half of the array remains sorted.** We can leverage this to perform a modified **Binary Search**.

1.  **Identify the Sorted Half:** In each step of the binary search, we compare `nums[middle]` with `nums[right]` (or `nums[left]`).
    -   If `nums[middle] <= nums[right]`: The **right half** (from `middle` to `right`) is sorted.
    -   If `nums[middle] > nums[right]`: The **left half** (from `left` to `middle`) is sorted.

2.  **Target Check:** Once we know which half is sorted, we check if the `target` lies within that sorted range.
    -   If the target is within the sorted range, we adjust our pointers to search exclusively in that half.
    -   Otherwise, we know the target must be in the *other* (unsorted) half, so we discard the sorted half and move our search there.

3.  **Repeat:** We continue narrowing down the search space until `left > right`.

### Implementation
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while(left <= right){
            int middle = left + (right - left) / 2;

            if(nums[middle] == target){
                return middle;
            }

            if(nums[middle] <= nums[right]){ // Direita ordenada
               if(nums[middle] <= target && target <= nums[right]){
                    left = middle + 1;
               }else{
                    right = middle - 1;
               }
            }else{ // Esquerda ordenada
                if(nums[left] <= target && target <= nums[middle] ){
                    right = middle - 1;
                }else{
                    left = middle + 1;
                }
            }
        }

        return -1;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of elements in the array.

-   **Time Complexity:** $O(\log n)$
    -   At each step, we discard half of the array, resulting in logarithmic time complexity.

-   **Space Complexity:** $O(1)$
    -   We only use a few variables (`left`, `right`, `middle`) for pointers, so the space used is constant.
