# <a href="https://leetcode.com/problems/reverse-nodes-in-k-group/" target="_blank">25. Reverse Nodes in k-Group</a>

### Problem Statement
Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return the modified list.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

### Approach
The main challenge is reversing sub-segments of the list while keeping the entire list connected correctly. When we reverse a group, the "old head" becomes the "new tail," and the "old tail" becomes the "new head." We need to ensure:
1.  The group *before* the current one points to the **new head**.
2.  The **new tail** of the current group points to the *next* group (even if that next group hasn't been reversed yet).

To manage this, we use pointers to track the boundaries:
1.  **Dummy Node:** We place a `dummy` node before the head to handle edge cases easily (like reversing the very first group).
2.  **`prevGroup`:** This pointer sits immediately before the group we are about to reverse.
3.  **Find `k`th Node:** In every iteration, we first traverse `k` steps to find the end of the current group (`kth`). If we reach `null` before finding `k` nodes, we stop (leaving the remainder as is).
4.  **Reversal Logic:**
    -   We record `nextGroup = kth.next`.
    -   We reverse the inner links of the group (standard linked list reversal).
    -   **Re-linking:**
        -   `prevGroup.next` is updated to point to the `kth` node (the new head of this segment).
        -   The old head (which is now the tail) is made to point to `nextGroup`.
    -   **Update Pointers:** We move `prevGroup` to the old head (the new tail) to prepare for the next iteration.

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
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0, head);
        ListNode prevGroup = dummy;
        while(true){
            ListNode kth = findKth(prevGroup, k);

            if(kth == null){
                break;
            }

            ListNode nextGroup = kth.next;

            ListNode prev = nextGroup;
            ListNode curr = prevGroup.next;
            while(curr != nextGroup){
                ListNode temp = curr.next;
                curr.next = prev;
                prev = curr;
                curr = temp;
            }

            ListNode temp = prevGroup.next;
            prevGroup.next = kth;
            prevGroup = temp;
        }

        return dummy.next;
    }

    public ListNode findKth(ListNode curr, int k){
        while(curr != null && k > 0){
            curr = curr.next;
            k--;
        }
        return curr;
    }
}
``` 

### Complexity Analysis
Let `N` be the number of nodes in the linked list.

-   **Time Complexity:** $O(N)$
    -   We traverse each node at most twice: once to find the `k`th node (`findKth`) and once to actually reverse the links. This results in linear time complexity.

-   **Space Complexity:** $O(1)$
    -   We only use a fixed number of pointers (`dummy`, `prevGroup`, `curr`, `prev`, etc.) regardless of the list size. The reversal is done in-place.
