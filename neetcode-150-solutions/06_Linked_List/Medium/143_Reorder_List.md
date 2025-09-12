# <a href="https://leetcode.com/problems/reorder-list/" target="_blank">143. Reorder List</a>

### Problem Statement
You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln
Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

### Approach
The solution can be broken down into three key steps:

1. **Find the middle of the list**
   Use the *slow and fast pointer* technique to locate the middle node. The slow pointer moves one step at a time, while the fast pointer moves two. When the fast pointer reaches the end, the slow pointer will be at the middle.

2. **Reverse the second half of the list**
   Starting from the middle, reverse the links of the nodes until the end of the list. This allows us to traverse from both ends toward the center.

3. **Merge the two halves**
   With two pointers (`left` starting from the head and `right` starting from the end), alternately link nodes from each side until the reordering is complete.

### Implementation
```java
class Solution {
    public void reorderList(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }

        ListNode previousNode = null;
        ListNode currentNode = slow;
        while(currentNode != null){
            ListNode nextNode = currentNode.next;
            currentNode.next = previousNode;
            previousNode = currentNode;
            currentNode = nextNode;
        }

        ListNode left = head;
        ListNode right = previousNode; // Last node
        while(right.next != null){
            ListNode nextLeft = left.next;
            ListNode nextRight = right.next;
            right.next = left.next;
            left.next = right;
            left = nextLeft;
            right = nextRight;
        }
        return;
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   We traverse the list up to three times: once to find the middle, once to reverse the second half, and once to merge. Each step is linear in the number of nodes.

-   **Space Complexity:** O(1)
    -   The algorithm uses a constant amount of extra space for a few variables. The space required does not grow with the size of the input.
