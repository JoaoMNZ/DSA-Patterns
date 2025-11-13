# <a href="https://leetcode.com/problems/remove-nth-node-from-end-of-list/" target="_blank">19. Remove Nth Node From End of List</a>

### Problem Statement
Given the `head` of a linked list, remove the `n`th node from the end of the list and return its head.

### Approach
This problem can be solved in a single pass using the **Sliding Window** technique. The goal is to find the node that comes *just before* the node we want to remove.

1.  We set up two pointers, `left` and `right`, both starting at the `head`.
2.  We first move the `right` pointer `n` steps forward. This creates a "gap" or window of size `n` between the two pointers.
3.  **Edge Case:** If `right` is `null` after this first loop, it means `n` is equal to the total number of nodes, so we need to remove the head. We just return `head.next`.
4.  **Sliding the Window:** If `right` is not `null`, we move both `left` and `right` pointers one step at a time until `right.next` is `null`.
5.  At this point, `right` is at the last node, and `left` is at the node *just before* the one we need to remove.
6.  We perform the removal by skipping the target node: `left.next = left.next.next`.

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode left = head;
        ListNode right = head;
        for (int i = 0; i < n; i++) 
            right = right.next;

        if (right == null) 
            return head.next;

        while (right.next != null) {
            right = right.next;
            left = left.next;
        }
        left.next = left.next.next;
        
        return head;
    }
}
``` 

### Complexity Analysis
Let `N` be the number of nodes in the linked list.

-   **Time Complexity:** O(N)
    -   We traverse the list once. The first loop moves `right` `n` steps, and the second loop moves `left` and `right` `N - n` steps.

-   **Space Complexity:** O(1)
    -   We only use a constant amount of extra space for the two pointers.
