# <a href="https://leetcode.com/problems/reverse-linked-list/" target="_blank">206. Reverse Linked List</a>

### Problem Statement
Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

### Approach
This is a standard linked list manipulation problem that can be solved iteratively using pointers. The goal is to change the `next` pointer of each node to point to its previous node instead of the next one.

We need two main pointers:
-   `current`: Starts at the `head` and moves through the list.
-   `previous`: Starts as `null` and will eventually become the new head of the reversed list.

The process is:
1.  Initialize `current = head` and `previous = null`.
2.  While `current` is not `null`:
    -   **Store the next node:** We need to save `current.next` in a temporary variable (`temp`) before we change it, otherwise we lose the rest of the list.
    -   **Reverse the pointer:** Set `current.next` to point to `previous`.
    -   **Move pointers forward:** Update `previous` to be the `current` node, and update `current` to be the `temp` node (the original next node).
3.  When the loop finishes (`current` becomes `null`), `previous` will be pointing to the last node of the original list, which is now the head of the reversed list.

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
    public ListNode reverseList(ListNode head) {
        ListNode current = head;
        ListNode previous = null;

        while(current != null){
            ListNode temp = current.next;
            current.next = previous;
            previous = current;
            current = temp;
        }
        
        return previous;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of nodes in the linked list.

-   **Time Complexity:** O(n)
    -   We iterate through the linked list exactly once.

-   **Space Complexity:** O(1)
    -   We only use a few pointer variables (`current`, `previous`, `temp`) regardless of the list size. The space required is constant.
