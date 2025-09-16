# <a href="https://leetcode.com/problems/add-two-numbers/" target="_blank">2. Add Two Numbers</a>

### Problem Statement
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

### Approach
This problem is like doing grade-school addition on paper, from right to left. Since the linked lists store digits in reverse order, we can process them from head to tail. The main challenge is handling the "carry-over".

1.  We'll iterate through both lists at the same time, using a `carry` variable initialized to `0`.
2.  In each step, we sum the values of the current nodes from `l1` and `l2`, plus the `carry` from the previous step. If one list is shorter than the other, we treat the value of the missing node as `0`.
3.  The value for our **new node** will be the last digit of this total sum ( `total % 10` ).
4.  The **new carry** for the next step will be the first digit of the total sum ( `total / 10` ).
5.  We continue this process until we have processed all nodes in both lists and there is no `carry` left over.

### Implementation
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode();
        ListNode currentNode = dummyHead;
        int carryOver = 0;
        
        while (l1 != null || l2 != null || carryOver != 0) {
            int value1 = (l1 != null) ? l1.val : 0;
            int value2 = (l2 != null) ? l2.val : 0;

            int total = value1 + value2 + carryOver;
            int digit = total % 10;
            carryOver = total / 10;
            currentNode.next = new ListNode(digit);

            currentNode = currentNode.next;
            l1 = (l1 != null) ? l1.next : null;
            l2 = (l2 != null) ? l2.next : null;
        }
        
        return dummyHead.next;
    }
}
``` 

### Complexity Analysis
Let `n` be the number of nodes in the longer of the two lists.

-   **Time Complexity:** O(n)
    -   We iterate through the linked lists once. The number of iterations is determined by the length of the longer list.

-   **Space Complexity:** O(1)
    -   The algorithm uses a constant amount of extra space for a few variables. The space taken by the new linked list returned as the result is not counted towards the auxiliary space complexity.
