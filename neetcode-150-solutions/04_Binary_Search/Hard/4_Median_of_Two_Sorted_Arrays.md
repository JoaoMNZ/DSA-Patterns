# <a href="https://leetcode.com/problems/median-of-two-sorted-arrays/" target="_blank">4. Median of Two Sorted Arrays</a>

### Problem Statement
Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the median of the two sorted arrays.

The overall run time complexity should be $O(\log (m+n))$.

### Approach
We need to partition both arrays such that the **Left Half** and **Right Half** of the combined set have equal lengths (or the left has one extra element), and every element in the Left Half is smaller than every element in the Right Half.



1.  **Binary Search on the Smaller Array:** To minimize operations, we perform the binary search on the smaller array (let's call it `A`). This ensures our complexity is $O(\log(\min(m, n)))$.
2.  **The Partition Logic:**
    * We make a cut in array `A` at index `i`.
    * We calculate the corresponding cut in array `B` at index `j` such that `i + j` equals half the total number of elements.
    * This gives us four boundary values:
        * $A_{left}$: The element just before the cut in A.
        * $A_{right}$: The element just after the cut in A.
        * $B_{left}$: The element just before the cut in B.
        * $B_{right}$: The element just after the cut in B.
3.  **Validation:** The partition is valid if and only if:
    * $A_{left} \le B_{right}$
    * $B_{left} \le A_{right}$
4.  **Adjusting the Binary Search:**
    * If $A_{left} > B_{right}$: The cut in A is too far to the right (value is too big). Move `right` pointer to `i - 1`.
    * If $B_{left} > A_{right}$: The cut in A is too far to the left (value is too small). Move `left` pointer to `i + 1`.
5.  **Result:** Once the valid partition is found:
    * If the total length is **odd**: The median is simply $\max(A_{left}, B_{left})$.
    * If the total length is **even**: The median is $(\max(A_{left}, B_{left}) + \min(A_{right}, B_{right})) / 2.0$.

### Implementation
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] A = nums1;
        int[] B = nums2;

        if(A.length > B.length){
            int[] temp = A;
            A = B;
            B = temp;
        }

        int total = A.length + B.length;
        int half = (total + 1) / 2;

        int left = 0;
        int right = A.length;
        while(left <= right){
            int Acut = (left + right) / 2;
            int Bcut = half - Acut;

            int Aleft = Acut > 0 ? A[Acut - 1] : Integer.MIN_VALUE;
            int Aright = Acut < A.length ? A[Acut] : Integer.MAX_VALUE;
            int Bleft = Bcut > 0 ? B[Bcut - 1] : Integer.MIN_VALUE;
            int Bright = Bcut < B.length ? B[Bcut] : Integer.MAX_VALUE;

            if(Aleft <= Bright && Bleft <= Aright){
                if(total % 2 == 0){
                    return (Math.max(Aleft, Bleft) + Math.min(Aright, Bright)) / 2.0;
                }
                return Math.max(Aleft, Bleft);
            }else if(Aleft > Bright){
                right = Acut - 1;
            }else{
                left = Acut + 1;
            }
        }

        return -1;
    }
}
``` 

### Complexity Analysis
Let `m` be the size of `nums1` and `n` be the size of `nums2`.

-   **Time Complexity:** $O(\log(\min(m, n)))$
    -   We perform a binary search on the smaller array (`A`). Since the search space is the size of the smaller array, the number of iterations is logarithmic relative to that size.

-   **Space Complexity:** $O(1)$
    -   We only use a fixed number of integer variables to store pointers and values.
