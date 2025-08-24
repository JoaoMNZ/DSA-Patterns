# <a href="https://leetcode.com/problems/linked-list-cycle/description/" target="_blank">141. Linked List Cycle</a>

### Problem Statement
Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

### Approach
This problem can be solved using the **Two Pointers** technique, often called **Floyd's Tortoise and Hare algorithm**.

Imagine two people running on a track. One is a "slow" runner who moves one step at a time, and the other is a "fast" runner who moves two steps at a time.
1.  We start both a `slow` pointer and a `fast` pointer at the `head` of the linked list.
2.  We move them in a loop: `slow` moves to the next node, and `fast` moves to the next, next node.
3.  If the list **does not have a cycle**, the `fast` pointer will eventually reach the end (`null`), and the loop will stop.
4.  If the list **does have a cycle**, the `fast` pointer will enter it first and start running in circles. Eventually, the `slow` pointer will also enter the cycle. Since the `fast` runner is moving faster, it is guaranteed to eventually catch up and meet the `slow` runner at the same node.

If the pointers ever meet, we have found a cycle and can return `true`. If the loop finishes because `fast` reached the end, there is no cycle, and we return `false`.

### Implementation
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            
            if (fast == slow) {
                return true;
            }
        }
        
        return false;
    }
}
``` 

### Complexity Analysis
- **Time Complexity:** O(n)
  - In the worst case, the pointers will traverse each of the `n` nodes in the list a constant number of times.

- **Space Complexity:** O(1)
  - We only use two pointers (`slow` and `fast`). The amount of memory used is constant and does not depend on the size of the list.
