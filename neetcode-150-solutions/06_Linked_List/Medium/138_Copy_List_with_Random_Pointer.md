# <a href="https://leetcode.com/problems/copy-list-with-random-pointer/" target="_blank">138. Copy List with Random Pointer</a>

### Problem Statement
A linked list of length `n` is given such that each node contains an additional `random` pointer, which could point to any node in the list, or `null`. Construct a **deep copy** of the list. The deep copy should consist of exactly `n` brand new nodes, where each new node has its value set to the value of its corresponding original node. The `next` and `random` pointers of the new nodes should point to the new nodes in the copied list.

### Approach
The main challenge here is connecting the `random` pointers correctly. When we create a copy of a node, its `random` pointer needs to point to the *copy* of another node, not the original. A `HashMap` is the perfect tool to keep track of this relationship.

This can be solved with a clear **two-pass approach**:

1.  **First Pass: Create Nodes and the Map**
    -   We iterate through the original linked list from head to tail.
    -   For each original node, we create a new node with the same value.
    -   We then store this pairing in our `HashMap`, mapping the original node to its new copy: `map.put(originalNode, copyNode)`. After this loop, our map acts as a bridge between the two lists.

2.  **Second Pass: Connect the Pointers**
    -   We iterate through the original list again.
    -   For each `originalNode`, we get its corresponding `copyNode` from the map.
    -   We then set the `next` and `random` pointers for our `copyNode`. We find the correct nodes to point to by looking them up in our map. For example: `copyNode.next = map.get(originalNode.next)`. This ensures all pointers in the new list point only to other new nodes.

Finally, the head of our new list is the value associated with the original `head` in the map.

### Implementation
```java
import java.util.Map;
import java.util.HashMap;

class Solution {
    public Node copyRandomList(Node head) {        
        Map<Node, Node> oldNodeToCopyNode = new HashMap<>();

        Node currentNode = head;
        while (currentNode != null) {
            Node copyNode = new Node(currentNode.val);
            oldNodeToCopyNode.put(currentNode, copyNode);
            currentNode = currentNode.next;
        }

        currentNode = head;
        while (currentNode != null) {
            Node copyNode = oldNodeToCopyNode.get(currentNode);
            copyNode.next = oldNodeToCopyNode.get(currentNode.next);
            copyNode.random = oldNodeToCopyNode.get(currentNode.random);
            currentNode = currentNode.next;
        }

        return oldNodeToCopyNode.get(head);
    }
}
``` 

### Complexity Analysis
-   **Time Complexity:** O(n)
    -   We iterate through the linked list of `n` nodes twice: once to create the copies and once to assign the pointers. This results in a time complexity of O(n) + O(n), which simplifies to O(n).

-   **Space Complexity:** O(n)
    -   We use a `HashMap` to store a mapping for each of the `n` nodes in the original list. Therefore, the space required grows linearly with the number of nodes.
