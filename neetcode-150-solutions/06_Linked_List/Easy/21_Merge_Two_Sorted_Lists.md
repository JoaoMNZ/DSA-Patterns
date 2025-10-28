# <a href="https://leetcode.com/problems/merge-two-sorted-lists/" target="_blank">21. Merge Two Sorted Lists</a>

### Problem Statement
You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

### Approach
This problem can be solved effectively using **Recursion**. The idea is to build the merged list by repeatedly comparing the current nodes of `list1` and `list2` and choosing the smaller one.

1.  **Base Cases:**
    -   If `list1` is empty, there's nothing left to merge from it, so the rest of the merged list is simply `list2`.
    -   Similarly, if `list2` is empty, the rest of the merged list is `list1`.
2.  **Recursive Step:**
    -   Compare the values of the current nodes of `list1` and `list2`.
    -   If `list1.val` is smaller or equal, we know this node comes next in the merged list. Its `next` pointer should point to the result of merging the *rest* of `list1` (`list1.next`) with the *entirety* of `list2`. We then return `list1` as the head of this merged portion.
    -   If `list2.val` is smaller, we do the opposite: `list2.next` should point to the result of merging `list1` with the *rest* of `list2` (`list2.next`). We return `list2`.

This process continues until one of the lists becomes empty, linking all nodes in sorted order.

### Implementation
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null) 
            return list2;
        if (list2 == null) 
            return list1;

        if(list1.val <= list2.val){
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        }else{
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
``` 

### Complexity Analysis
Let `m` be the number of nodes in `list1` and `n` be the number of nodes in `list2`.

-   **Time Complexity:** O(m + n)
    -   The algorithm visits each node from both lists exactly once.

-   **Space Complexity:** O(m + n)
    -   The space complexity is determined by the maximum depth of the recursion stack. In the worst case, the recursion goes as deep as the total number of nodes in both lists.
