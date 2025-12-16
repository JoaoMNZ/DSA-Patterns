# <a href="https://leetcode.com/problems/merge-k-sorted-lists/" target="_blank">23. Merge k Sorted Lists
</a>

### Problem Statement
You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

### Approach
To merge `k` sorted lists efficiently, we always need to know which of the `k` current nodes is the smallest. A **Min-Heap** (Priority Queue) is the perfect data structure for this because it allows us to retrieve the minimum element in $O(1)$ and insert a new element in $O(\log k)$.


1.  **Initialization:** We traverse the `lists` array and add the **head** of each non-null list into the Min-Heap. The heap will order them based on their values.
2.  **Processing:**
    -   We extract (`poll`) the node with the smallest value from the heap. This node becomes the next node in our result list.
    -   **Next Step:** If the extracted node has a `next` node (i.e., the list it came from is not empty), we add that `next` node into the heap.
3.  **Termination:** We repeat this process until the heap is empty, meaning all nodes from all lists have been processed.

### Implementation
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */

import java.util.Queue;
import java.util.PriorityQueue;
import java.util.Comparator;

class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        Queue<ListNode> minHeap = new PriorityQueue<>(Comparator.comparing(n -> n.val));
        
        for(ListNode list : lists){
            if(list != null){
                minHeap.offer(list);
            }
        }

        ListNode res = new ListNode();
        ListNode curr = res;

        while(!minHeap.isEmpty()){
            ListNode node = minHeap.poll();
            curr.next = node;
            curr = curr.next;

            if(node.next != null){
                minHeap.offer(node.next);
            }
        }

        return res.next;
    }
}
```

### Complexity Analysis
Let `n` be the total number of nodes across all lists, and `k` be the number of linked lists.

-   **Time Complexity:** $O(n \log k)$
    -   We iterate through every single node (`n` nodes total). For each node, we perform a heap insertion and deletion, which takes $O(\log k)$.

-   **Space Complexity:** $O(k)$
    -   The Min-Heap stores at most `k` nodes at any one time.
